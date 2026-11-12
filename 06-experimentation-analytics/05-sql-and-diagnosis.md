# 05｜广告 SQL：指标计算、JOIN 重复与经营诊断

## 一、数据分析的第一步是确认粒度（Grain）

教学用三张表：
- ad_daily：一行代表 report_date × ad_id，有 impressions、clicks、spend_usd；
- ad_creative_map：一行代表 ad_id × creative_id；
- conversion_event：一行代表一个转化 event_id，并关联 ad_id 和时间。

**典型错误**：一条广告关联三个创意时，ad_daily JOIN ad_creative_map 产生三行，随后 SUM(spend_usd) 会把广告级花费重复三次。

## 二、先在一致的广告日粒度聚合

~~~sql
SELECT
    report_date,
    SUM(impressions) AS impressions,
    SUM(clicks) AS clicks,
    SUM(spend_usd) AS spend_usd,
    1.0 * SUM(clicks) / NULLIF(SUM(impressions), 0) AS ctr,
    1000.0 * SUM(spend_usd) / NULLIF(SUM(impressions), 0) AS cpm,
    1.0 * SUM(spend_usd) / NULLIF(SUM(clicks), 0) AS cpc
FROM ad_daily
GROUP BY report_date;
~~~

这是通用 SQL 教学语法。整数除法、时间函数和 NULLIF 在不同数据库方言中需要适配。

**为什么不能直接平均每条 Ad 的 CTR？** 因为广告曝光量不同。例：A 曝光100、点击10，CTR10%；B 曝光900、点击9，CTR1%；总体 CTR=(10+9)/(100+900)=**1.9%**，不是两个比例的算术均值5.5%。

## 三、正确处理一对多素材映射

如果目标是广告级收入或花费，可以先在素材关系表聚合，确保每个 ad_id 最多一行，再合并：

~~~sql
WITH creative_counts AS (
    SELECT ad_id, COUNT(DISTINCT creative_id) AS creative_count
    FROM ad_creative_map
    GROUP BY ad_id
)
SELECT
    d.report_date,
    SUM(d.spend_usd) AS spend_usd,
    SUM(d.impressions) AS impressions,
    COUNT(DISTINCT d.ad_id) AS ads
FROM ad_daily d
LEFT JOIN creative_counts c
    ON d.ad_id = c.ad_id
GROUP BY d.report_date;
~~~

如果 ad_daily 本身也含多个子粒度，例如位置×时间，则必须先按目标统计单元处理，不能认为 CTE 自动解决所有重复。

如果真正需要 Creative 级效果，应该优先使用官方支持的 Creative 报表；没有明确的花费/展示归属口径时，不能随意把 Ad 花费等额复制到每个关联 Creative。

## 四、如何做广告收入同比诊断？

第一层：统一金额口径、币种、汇率、账期、曝光计数规则和过滤范围。

第二层：用 \(R=I/1000\times eCPM\) 分解总量、单价、交互，再按国家、广告样式、客户类型做结构分解。

第三层：寻找预算需求、库存、竞价、投放交付或数据链路中的真实瓶颈。

第四层：提出可验证的产品方案，结合实验和用户体验护栏评估；描述性差异不等于因果关系。

## 五、简历项目的迁移：域名—广告主反查

在 GMPT 风控分析等场景中，可以用同样的数据粒度思想：Domain、Ad、Account 往往是多对多或时间变化的关系。查询前确定是否统计风险域名数、关联 Ad 数、广告主账户数、实际处置数。跨层级去重避免把一条命中重复当多条独立风险。

更详细示例见 [对象层级、SQL 与种子样本](../01-advertising-fundamentals/10-object-granularity-and-risk-sampling.md)。

## 面试题

**问**： JOIN 后某市场广告收入翻倍，是流量暴涨吗？

**答**：先检查 JOIN 键、关联基数、对象粒度和金额重复计数。只有排除数据错误后，才能解释实际业务变化。

[上一节](04-attribution-and-lift.md) · [下一节](06-experiment-risks.md) · [目录](README.md)
