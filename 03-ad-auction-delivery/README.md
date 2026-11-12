# 03 · Ad Auction, Delivery & Traffic Allocation｜竞价、投放与流量分配

本章讨论广告平台的一个核心任务：**在有限的用户注意力与广告机会下，选择哪些广告参与、怎样评估候选价值、如何执行预算与交付，并兼顾用户体验及广告主目标**。

| 顺序 | 内容 | 核心问题 |
| --- | --- | --- |
| [01 合格库存与候选准入](01-eligibility-and-inventory.md) | 广告进入竞价前发生什么？ | 哪些请求/广告根本不应参与分配？ |
| [02 竞价排序与计价原理](02-auction-ranking-and-pricing.md) | 出价、预估效果、质量怎样互动？ | 出价最高一定赢吗？ |
| [03 出价策略与优化目标](03-bidding-and-optimization.md) | CPM、CPC、CPA、oCPM、Cost Cap | 广告主如何控制成本？ |
| [04 投放节奏与学习阶段](04-pacing-and-learning.md) | 预算、频次、学习、稳定性 | 为什么花不出去？ |
| [05 预订与竞价流量分配](05-reservation-and-allocation.md) | 品牌交付承诺与竞价机会成本 | 高价竞价为何不必然优先？ |
| [06 投放异常与可观测性](06-delivery-troubleshooting.md) | 请求、响应、展示、测量各层诊断 | 怎样从用户投诉找到系统问题？ |
| [07 计算题与面试演练](07-worked-cases.md) | 完整计算、业务诊断与产品设计 | 如何形成可执行方案？ |

## 先区分四个系统问题

~~~mermaid
flowchart LR
 A["合规可用库存"] --> B["候选准入及约束"]
 B --> C["竞价/预订分配"]
 C --> D["交付与计量"]
 D --> E["客户效果及用户反馈"]
 E -. "模型/策略改进" .-> B
~~~

**竞价是一种交易机制，不等于广告平台的全部。** 购买、库存预测、质量政策、竞价、预算节奏和展示成功需要分别定义。

本章包含的分数与优化公式是**教学构造**。TikTok、Google 等公开文档只用于说明公开机制或产品选项，绝不将假设算法当作内部实现。

[返回首页](../README.md) · [上一章：广告经济学](../02-advertising-economics-and-monetization/README.md) · [下一章：广告产品与创意](../04-ad-products-creative/README.md)
