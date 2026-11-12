# 10｜对象粒度、SQL 统计与风险种子样本：从 Video 命中到 Ad / Advertiser 归因

**Data Grain, Entity Mapping & Seed Sample Design｜面向广告数据分析、策略产品及风控面试**

本篇承接 [09 广告对象层级](09-ad-object-hierarchy.md)，专门解决三个问题：

1. 为什么一条视频命中风险，可能关联多条 Ad 和多个 Advertiser？
2. 为什么统计风险覆盖量时，COUNT(*)、COUNT(DISTINCT ad_id) 和 COUNT(DISTINCT video_id) 可能差别很大？
3. 设计种子样本平台时，如何选择样本单位、处理复用与重复，并避免评估数据泄漏？

**所有实体 ID、表名、SQL 与数字均为教学虚构，不是 TikTok 或 ByteDance 内部数据库或处置策略。**

## 1. 第一原则：任何数据表先说清楚 Grain（最小粒度）

Grain 指一行数据代表的最小业务单元。下面几张表可能同时存在，却不能用同一个 COUNT(*) 比较。

| 教学表名 | 一行代表什么 | 主键示例 | 常见用途 |
| --- | --- | --- | --- |
| ad_snapshot | 某个时刻/版本的广告配置 | ad_id + snapshot_time | 确认当时广告引用的创意与落地页 |
| ad_asset_relation | 一条广告与一个媒体资产的关联 | ad_id + asset_id + valid_from | 找出同一视频关联哪些广告 |
| creative_entity | 一个创意对象或配置 | creative_id | 创意管理与诊断 |
| video_asset | 一个视频素材 | video_id | 去重与视频级识别 |
| landing_page_snapshot | 某 URL 在某时点的页面快照 | snapshot_id | 页面风险检测及留证 |
| risk_model_result | 模型一次判断及其输出 | inference_id | 风险模型效果、FP/FN 复盘 |
| enforcement_action | 一次经过授权的风险处置 | action_id | 分析实际拦截/审核，而非只统计风险命中 |
| impression_fact | 一次有效曝光事件 | event_id | 广告交付与效果报表 |

**注意时间维度**：如果相同 Ad 在不同时间替换过素材，那么视频风险结果不一定适用于它的全部历史时期。查询中需要满足关联生效时间、广告投放状态以及模型检测时间窗。

## 2. 为什么一条风险视频可能关联多个广告？

假设构造了以下**虚构有效关系**：

| video_id | ad_id | advertiser_id | 是否当前有效 |
| --- | --- | --- | --- |
| V001 | AD01 | ACCOUNT_A | 是 |
| V001 | AD02 | ACCOUNT_A | 是 |
| V001 | AD03 | ACCOUNT_B | 是 |
| V002 | AD04 | ACCOUNT_A | 是 |
| V001 | AD05 | ACCOUNT_C | 否：历史引用 |

若模型只对 V001 命中一次，且我们只统计当前有效关系，则：

| 指标 | 正确数量 | 解释 |
| --- | ---: | --- |
| 风险素材数 | 1 | COUNT DISTINCT video_id |
| 当前关联广告数 | 3 | COUNT DISTINCT ad_id |
| 当前关联账户数 | 2 | COUNT DISTINCT advertiser_id |
| 当前风险广告“已确认违规”数 | 无法直接给出 | 模型命中不代表人工或规则确认 |
| 当前已封禁账户数 | 无法直接给出 | 命中并不等于处置 |

如果使用 COUNT(*) 对联表后的结果统计“风险视频”，可能把 1 个视频算成 3 或 4 个样本。

**这意味着指标首先需要定义对象层级，再谈增长、下降或策略覆盖。**

## 3. SQL：一个命中如何映射到多个对象

假设某个教学系统有两张表：

- risk_video_result(video_id, detected_at, risk_label, predicted_positive)
- ad_video_relation(ad_id, advertiser_id, video_id, valid_from, valid_to)

下面的 SQL 只用于解释去重和时效约束，并非可直接连接生产数据库的查询。

~~~sql
WITH hits AS (
    SELECT
        video_id,
        detected_at,
        risk_label
    FROM risk_video_result
    WHERE predicted_positive = 1
      AND risk_label = 'EXAMPLE_RISK'
),
valid_mapping AS (
    SELECT
        h.video_id,
        h.risk_label,
        m.ad_id,
        m.advertiser_id
    FROM hits h
    JOIN ad_video_relation m
      ON h.video_id = m.video_id
     AND h.detected_at >= m.valid_from
     AND (m.valid_to IS NULL OR h.detected_at < m.valid_to)
)
SELECT
    COUNT(DISTINCT video_id) AS flagged_video_count,
    COUNT(DISTINCT ad_id) AS related_ad_count,
    COUNT(DISTINCT advertiser_id) AS related_account_count
FROM valid_mapping;
~~~

这里演示的是**在检测时刻生效的映射**。如果业务问题改成“过去30天内被风险素材影响过的所有实际曝光广告”，还需要关联广告真实投放和曝光事实，并按窗口去重，不可直接套用上式。

另一个容易遗漏的问题：某些广告可能没有 Video，而是只有 Image、Post 或其他素材，因此**只用 Video 关系表不能覆盖全部广告内容**。

## 4. 广告效果数据的粒度陷阱

如果你用 Ad 级曝光、花费去 JOIN 多条视频/创意关系，然后直接 SUM(spend)，很可能把同一广告花费重复累计。

例：Ad A 花费 100 美元，包含 2 条关联素材。如果联表产生两行后执行 SUM(spend)，得到 200 美元，这不是收入或花费真的翻倍。

正确做法是在聚合前确定：

- 目标是**Ad 级业务指标**，还是素材级分摊/归因？
- 是否存在“一条 Ad 多个 Creative”的结构，或动态创意组合？
- 是否有官方提供的 Creative 级指标及测量定义？
- 是否需要先在 Ad 粒度聚合，再按明确分配规则关联素材？

不能对“一个 Ad 被两个 Creative 使用”自作主张将广告全部成本各算一遍，也不能默认平均分摊真实曝光。特别是升级 Smart+ 的 Creative 级报告，应优先遵循该产品明确的报表对象与指标口径。

## 5. 风险识别（Detection）与风险处置（Enforcement）要分开建模

~~~mermaid
flowchart TD
    A["内容或页面对象：Video / Image / Landing Page"] --> B["Model Result：风险分数与标签"]
    B --> C["Identity Mapping：Ad / Account / Domain 关联"]
    C --> D{"Policy Decision<br/>证据、规则与授权"}
    D --> E["放行 / 复核 / 限制等动作"]
    E --> F["结果确认、申诉、复盘"]
~~~

这只是风险产品设计的概念图，**不等于任何真实公司的在线链路或规则顺序**。

模型命中数、风险广告关联数、实际处置数、确认违规数分别属于不同阶段。报告中的“累计命中 10 万风险广告”不能在没有额外证据的情况下扩展为“成功拦截 10 万条广告”或“节省了相当于 10 万次损失”。

同样，一条 Video 在多个广告中出现，不代表这些广告全部应该被认定为风险；不同广告配置、受众、落地页和合法授权背景可能影响判定。

## 6. 种子样本究竟以 Video、Creative 还是 Ad 为单位？

**没有统一的正确答案。要先看模型需要预测什么。**

| 识别任务 | 样本主对象可能是什么 | 设计重点 |
| --- | --- | --- |
| 视频视觉内容风险 | video_id 或受控的内容快照 | 同视频重复投放时避免重复计入 |
| 图片视觉内容风险 | image_id | 相同图片跨广告复用与近重复 |
| 创意整体风险 | creative_id 或 ad+creative 组合 | 需要完整结合素材、文案、CTA |
| 广告-落地页信息不一致 | ad_id + landing_snapshot_id | 判断必须同时观察两侧内容 |
| 落地页风险 | URL 快照或页面实体 | 页面变化、重定向、抓取时间 |
| 账户级风险评估 | account_id + 时间窗 + 证据集合 | 不是一个视频标签自动推出账户违规 |

例如，如果只识别视频里是否出现某个风险元素，那么 Video 是自然的识别单位；如果要判断“视频宣传与结算页实际价格是否不一致”，**只靠 video_id 就不足以表达标签**，需要至少关联广告和当时页面快照等上下文。

### 一个合理的样本平台产品模型

| 字段（教学示例） | 作用 |
| --- | --- |
| sample_id | 样本条目唯一 ID |
| object_type | VIDEO、IMAGE、AD、LANDING_SNAPSHOT 等 |
| object_id / composite_key | 业务对象 ID 或复合键 |
| snapshot_at | 采样与内容快照时间 |
| risk_label_id + version | 标签及定义版本 |
| label_status | 正例、负例、待判定/有争议 |
| evidence_ref | 合规、可审计的判断依据 |
| source | 获样来源 |
| split | train / validation / holdout（用途区分） |
| reviewer + decision_time | 复核信息与时间 |

不应在公开学习仓库中保存真实账户 ID、违规视频、客户数据或非公开处置规则；上述字段只是说明产品设计方法。

## 7. 如何设计样本去重与数据集划分？

先明确去重目标：

**精确去重**：同一媒体资产 ID、同一内容快照或一致的内容指纹。

**近重复检查**：视频重编码、裁剪、加字幕、缩放等可能令不同素材在内容上高度相似；应通过合规的近重复检测方法辅助核实，不可直接认定所有相似内容的风险标签相同。

**跨广告关联**：相同 Video 出现在多条 Ad 中时，若要评估 Video 分类模型，不能把这些广告作为相互独立的素材样本。

**数据集隔离**：对可能重复或高度相似的素材，尽量按合适的实体组进行训练/验证/最终测试拆分，避免同一内容同时进入训练与测试，导致效果虚高。

**时间泄漏**：风险标签发生变化、页面被修改，或者结算信息是若干天后才获得的，不能拿未来信息当作过去的模型输入。

种子样本集可以特意收集“典型正例、困难负例、边界案例”来覆盖风险模式；**但这种富集抽样不自动等于线上真实分布，不能直接用它声称线上 Precision 是多少。**

## 8. 面试官可能怎样连续追问？

### 题目 A：同一视频被 10 条广告使用，应该制作 10 份种子样本吗？

**参考回答**：如果任务只是视频内容识别，应优先以 Video 或内容实体为单位建立种子样本，并保留 Ad 关系，避免重复样本导致权重偏移和训练/测试泄漏。如果任务是“广告加落地页”风险判断，则同一视频对应不同广告、不同链接，可能需要分别记录上下文组合。样本主键应由任务定义决定，而不是只根据业务表有多少行。

### 题目 B：模型命中 100 条 Video，能不能报告命中 100 条广告？

**参考回答**：不能直接说。先把 Video 映射到指定时间内真实引用它的 Ad，再按 Ad 去重统计；一条视频可能关联多条广告，也可能当前没有活跃广告。还要区分模型命中与确认违规或真实拦截。

### 题目 C：如果某域名关联上千个广告，要不要全部拦截？

**参考回答**：不能仅由关联规模决定。需要先明确域名风险证据和时效、广告与具体 URL 的关系、是否存在合法共用平台或误关联，再依经过授权的策略确定需复核的粒度与处置动作，设置误判控制和申诉机制。不公开或臆测生产系统的实际触发阈值。

### 题目 D：从 Ad 粒度分析到 Creative 粒度，广告花费为什么可能翻倍？

**参考回答**：通常需要先排查一对多 JOIN 导致的指标重复累加，以及当前广告产品是否提供 Creative 级真实拆分报表。不能按 Ad 的完整 Spend 直接分配给每条关联 Creative。

## 9. 核心术语速查

**Account/Advertiser**：账户和商务广告主概念可能不同。  
**Campaign**：广告系列。  
**Ad Group**：广告组。  
**Ad**：投放广告对象。  
**Creative**：创意概念、广告创意实例或特定 Smart+ 报表对象，需结合语境。  
**Asset / Video / Image / Post**：素材或内容资产。  
**Landing Page / URL / Domain**：站外转化与风险证据相关实体，不等于 Ad 子对象。  
**Grain**：一条记录代表的最小业务单位。  
**Deduplication**：按指定实体、窗口和规则去重。  
**Lineage**：识别结果与最终处置之间的证据和版本链路。  
**Snapshot**：某个时点对象内容及配置的记录。  
**Label Leakage**：模型训练或评估中使用了当时不可获得的目标信息。

## 10. 官方资料

- [TikTok Ads Structure](https://ads.tiktok.com/resources/help/article/tiktok-ads-structure)
- [Upgraded Smart+ Reporting（含 Creative ID）](https://ads.tiktok.com/resources/help/article/about-reporting-for-the-upgraded-smart-experience)
- [TikTok API for Business — Reporting dimensions](https://business-api.tiktok.com/gateway/docs/index?doc_id=1751443956638721)
- [TikTok Spark Ads（Post 及授权）](https://ads.tiktok.com/resources/help/article/spark-ads?lang=en)

**核对日期：2026-11-12。** 本篇 SQL、数据模型和面试案例为通用教学设计；不能将其归因于任何未公开内部系统。

[上一篇：广告对象层级](09-ad-object-hierarchy.md) · [返回第一章](README.md)
