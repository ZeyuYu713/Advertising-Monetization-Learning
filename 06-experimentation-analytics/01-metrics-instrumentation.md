# 01｜广告指标、埋点与数据字典

## 一、先把指标的分子、分母、窗口说清楚

| 指标 | 常见定义 | 最容易犯的错误 |
| --- | --- | --- |
| CTR | Clicks / Impressions | 未核对点击的去重与有效口径 |
| CVR | Conversions / Clicks（某些场景） | 不说明是否以曝光、点击还是会话为分母 |
| CPC | Spend / Clicks | 把实际平均成本和出价等同 |
| CPA | Spend / Conversions | 漏掉转化回传延迟或事件口径 |
| CPM | Spend / Impressions ×1000 | 以为实际计费一定是 CPM |
| eCPM | Revenue / Impressions ×1000 | 收入财务口径不一致 |
| ROAS | Attributed Revenue / Spend | 误当增量利润率 |
| Reach | 指定窗口独立触达 | 不明确跨设备/跨产品去重 |
| Frequency | Impressions / Reach | 只看平均频次 |
| Delivery Rate | 实际交付 / 约定交付 | 把预测值当合同承诺 |

## 二、一个概念化事件链路

~~~mermaid
flowchart LR
 A["Ad Opportunity"] --> B["Ad Request"]
 B --> C["Ad Response"]
 C --> D["Measured Impression"]
 D --> E["Click / View"]
 E --> F["Conversion Event"]
 F --> G["Attribution"]
~~~

这不是生产架构。实际系统可能预加载、缓存、过滤无效流量、做隐私处理或延迟上报，必须按实际报表指标确认。

## 三、所有事实表必须有 Grain

例如广告日表的一行可能是 日期×ad_id，而素材映射表一行是 ad_id×creative_id。两表直接 JOIN 后按日 SUM(spend)，若一个 Ad 对应多素材，就可能把同一笔花费重复计算。

参考 [对象粒度与 SQL](../01-advertising-fundamentals/10-object-granularity-and-risk-sampling.md)。

## 四、跨国广告数据的问题

时区、汇率、退款、税、财务入账、归因窗口和回传延迟，会让“昨天与今天 CPA 比较”不稳定。假设某广告昨日花费$1000，当前显示20次转化，CPA=$50；两天后回传总数25，CPA=$40。这个变化可能只是标签补齐，而非策略突然变好。

## 五、完整计算

曝光150万、点击3万、归因转化600、花费$12,000：

$$
CTR=30000/1500000=2\%,\quad CPM=12000/1500000\times1000=\$8
$$

$$
CPC=12000/30000=\$0.40,\quad CVR=600/30000=2\%,\quad CPA=12000/600=\$20
$$

这里 CVR 以点击为分母，转化事件须用本题一致的归因窗口。

## 六、PM 的产品切入点

建设统一口径的数据字典、可按 Ad/Creative/Market 下钻的指标视图、数据刷新时间、异常提示、权限审核与指标血缘。验收不只看报表点击率，更要看排障时间、错判下降及客户信任。

[下一节](02-ab-testing-design.md) · [目录](README.md)
