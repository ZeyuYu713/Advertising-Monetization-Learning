# 06 · Experimentation & Analytics｜广告实验、归因与数据分析

本章回答：**广告收入或转化增长是否由产品改动导致？** 如何设计实验、解释统计不确定性、校验报表口径，并避免归因、样本偏差和重复计算？

| 章节 | 主要知识 |
| --- | --- |
| [01 指标体系与事件口径](01-metrics-instrumentation.md) | Impression、Reach、Spend、Conversion 与埋点 |
| [02 A/B实验设计](02-ab-testing-design.md) | 假设、随机单位、MDE、护栏、功效 |
| [03 统计检验与样本量](03-statistics-and-power.md) | 比例差、置信区间、SRM |
| [04 归因与增量研究](04-attribution-and-lift.md) | Attribution、Brand Lift、Conversion Lift |
| [05 SQL与数据粒度](05-sql-and-diagnosis.md) | 去重、漏斗、分层与数据诊断 |
| [06 实验风险与长期价值](06-experiment-risks.md) | 干扰、早停、多重比较、回滚 |
| [07 完整计算与面试题](07-worked-cases.md) | 十道含过程与结果的业务题 |

~~~mermaid
flowchart LR
 A["业务问题"] --> B["指标定义"]
 B --> C["实验设计"]
 C --> D["埋点及SRM核查"]
 D --> E["效应和区间估计"]
 E --> F["护栏与长期影响"]
 F --> G["上线/回滚"]
~~~

**边界**：TikTok Ads Manager 的广告主 Split Testing 与平台内部产品 A/B 实验的随机化对象、结果解释不一定相同。所有数值为虚构教学数据，不能将广告主报表归因与真实因果增量混淆。

[返回首页](../README.md) · [下一章](../07-product-cases-interviews/README.md)
