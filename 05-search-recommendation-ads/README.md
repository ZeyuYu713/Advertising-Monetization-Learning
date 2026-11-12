# 05 · Search, Recommendation & Ads｜搜索、推荐与广告协同

搜索、推荐和广告看似都在“排序”，实际目标和数据结构有区别。搜索需要尊重用户的显式查询意图，推荐更多利用上下文与历史反馈，广告系统还需满足客户预算、合规准入、交易机制与商业结果。平台不能把这三者简单合并成“谁的点击率高就排前面”。

| 顺序 | 专题 | 业务问题 |
| --- | --- | --- |
| [01 搜索推荐广告的三套系统](01-three-ranking-systems.md) | 三者的目标、候选与数据有什么区别？ |
| [02 搜索广告与意图匹配](02-search-ads-intent.md) | Search Ads、Automatic Search Placement、TopSearch 怎样区分？ |
| [03 关键词、搜索词与报告粒度](03-keywords-and-query-reporting.md) | Broad、Exact、Negative、Impression Share 怎样解释？ |
| [04 信息流推荐与广告插入](04-feed-ranking-and-ad-insertion.md) | 广告怎样与自然内容共存？ |
| [05 多目标与长期生态](05-multiobjective-and-longterm-value.md) | 点击收益、广告收入、用户满意度怎样权衡？ |
| [06 信号反馈、归因与偏差](06-feedback-attribution-and-bias.md) | 哪些数据能训练排序？哪些指标不能直接比较？ |
| [07 实战案例](07-worked-cases.md) | 搜索广告低消耗、推荐负载、跨层级分析怎么回答？ |

## 三套排序与商业约束

~~~mermaid
flowchart LR
 A["用户请求或搜索 Query"] --> B["自然内容候选"]
 A --> C["合格广告候选"]
 B --> D["相关性/消费体验排序"]
 C --> E["购买约束/广告价值排序"]
 D --> F["展示与重排约束"]
 E --> F
 F --> G["用户行为、客户效果与平台收益"]
~~~

这是用于理解的抽象图，不代表 TikTok 具体召回/排序、广告插入或混排实现。

**背景材料**：TikTok 2026 年官方说明区分 Search Ads Campaign、Automatic Search Placement 与 TopSearch，其中市场和账户可用性不同；请以各官方页面为准。

[返回首页](../README.md) · [上一章](../04-ad-products-creative/README.md) · [下一章](../06-experimentation-analytics/README.md)
