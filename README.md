# Advertising & Monetization Learning

**数字广告、品牌广告、广告经济学、竞价交付、广告产品、搜索推荐、实验分析与产品经理面试知识库**

Digital Advertising · Brand Advertising · Monetization · AdTech · Product Strategy

本项目按照**广告主为什么付费 → 广告业务如何交易和交付 → 平台如何组织内容与广告流量 → 怎样评估增量收益 → 如何设计可落地的商业产品**组织。所有章节包含概念定义、关键指标、典型业务问题及练习，不把虚构案例和平台内部实现混为一谈。

## 全站学习导航

| 模块 | 内容简介 | 学习入口 |
| --- | --- | --- |
| **01 · Advertising Fundamentals** | 广告生态、品牌与效果、投放结构、CPM、竞价基础、对象层级、SQL粒度和种子样本 | [第一章：广告基础](01-advertising-fundamentals/README.md) |
| **02 · Advertising Economics & Monetization** | 收入价值链、广告库存、量价结构、客户预算、定价、广告负载、增长与经营分析 | [第二章：广告经济学与变现](02-advertising-economics-and-monetization/README.md) |
| **03 · Ad Auction, Delivery & Traffic Allocation** | 广告准入、竞价排序、出价优化、预算节奏、R&F预订、交付和故障诊断 | [第三章：竞价与流量分配](03-ad-auction-delivery/README.md) |
| **04 · Ad Products & Creative Strategy** | Format、Placement、Creative、Asset、创意制作流程、新广告产品PRD和品牌测量 | [第四章：广告产品与创意策略](04-ad-products-creative/README.md) |
| **05 · Search, Recommendation & Ads** | 搜索意图与关键词、Search Ads、TopSearch、信息流混排、多目标与反馈偏差 | [第五章：搜索、推荐与广告](05-search-recommendation-ads/README.md) |
| **06 · Experimentation & Analytics** | 指标字典、A/B实验、样本量、置信区间、SRM、Brand/Conversion Lift、SQL和归因 | [第六章：实验与数据分析](06-experimentation-analytics/README.md) |
| **07 · Product Case Studies & Interviews** | 收入下降、低消耗、新品PRD、推荐流策略、统计、行为面试、英文问答与模拟试卷 | [第七章：产品案例与面试](07-product-cases-interviews/README.md) |
| **专题 · Brand Advertising Deep Dive** | TikTok品牌广告产品矩阵、TopView/TopReach/Pulse/Spark、R&F、百万美元规划和Brand Lift | [品牌广告进阶专题](brand-advertising/README.md) |

**上述七章及品牌广告专题均已有可直接阅读的正式笔记**，不再是仅有计划或占位目录。

## 最适合从哪里开始？

**此前没有系统学过广告业务**：01 基础 → 02 经济学 → 03 竞价与交付 → 04 广告产品 → 05 搜索与推荐 → 06 实验 → 07 案例。

**准备国际化品牌广告商业产品面试**：品牌广告专题 → [04 产品与创意](04-ad-products-creative/README.md) → [02 广告经营](02-advertising-economics-and-monetization/README.md) → [03 竞价/预订交付](03-ad-auction-delivery/README.md) → [07 案例面试](07-product-cases-interviews/README.md)。

**准备广告流量、搜索策略产品面试**：[05 搜索推荐](05-search-recommendation-ads/README.md) → [03 竞价与流量分配](03-ad-auction-delivery/README.md) → [06 实验分析](06-experimentation-analytics/README.md) → [07 案例](07-product-cases-interviews/README.md)。

**结合 TikTok Ads 广告风控和数据产品经历**：[09 广告对象层级](01-advertising-fundamentals/09-ad-object-hierarchy.md) → [10 数据粒度、SQL与种子样本](01-advertising-fundamentals/10-object-granularity-and-risk-sampling.md) → [06 SQL与实验](06-experimentation-analytics/README.md)。

## 第一章：快速索引

| 主题 | 链接 |
| --- | --- |
| 广告生态与商业模式 | [01](01-advertising-fundamentals/01-ecosystem-and-business-model.md) |
| 品牌广告与效果广告 | [02](01-advertising-fundamentals/02-brand-vs-performance.md) |
| 广告投放完整链路 | [03](01-advertising-fundamentals/03-campaign-lifecycle.md) |
| 核心指标与预算 | [04](01-advertising-fundamentals/04-metrics-and-budget.md) |
| 购买、计费与出价 | [05](01-advertising-fundamentals/05-buying-and-bidding.md) |
| 样式、创意与素材 | [06](01-advertising-fundamentals/06-formats-and-creatives.md) |
| 广告效果、实验与产品思维 | [07](01-advertising-fundamentals/07-measurement-and-product-thinking.md) |
| 基础计算与案例 | [08](01-advertising-fundamentals/08-worked-cases.md) |
| Account、Campaign、Ad Group、Ad、Creative、Video | [09](01-advertising-fundamentals/09-ad-object-hierarchy.md) |
| 对象去重、SQL、风险样本和跨层级关联 | [10](01-advertising-fundamentals/10-object-granularity-and-risk-sampling.md) |

## 从商业问题出发的知识地图

~~~mermaid
flowchart LR
 A["客户目标与媒体预算"] --> B["广告产品与Creative"]
 B --> C["合格库存、竞价与预订"]
 C --> D["实际交付与用户体验"]
 D --> E["转化、品牌效果与增量"]
 E --> F["收入、复投和产品迭代"]
 F -. "改进产品价值" .-> A
~~~

阅读时要始终区分：
- **Ad Account、Campaign、Ad Group、Ad、Creative、Video 等对象层级**，以及它们可能存在的多对多关联；
- **Buying Type、Billing、Bidding Strategy、Objective**，这些并非一个概念；
- **广告主实际花费、平台财务收入、归因转化和因果增量**，不能不经核对直接互换；
- **短期广告收入和长期用户价值**，广告负载提升不一定增加长期净收益。

## 公式与案例规范

涉及计算时必须标明分子、分母、货币单位、对象粒度及时间窗。GitHub Markdown 使用标准表格、Mermaid 和 `$$` 块级数学公式。不同数据报表的 Fill Rate、eCPM、CVR 等同名字段可能具有不同定义。

文中的客户、预算、收入、价格、转化、库存和实验样本均为教学虚构数据，除非明确引用公开统计或官方定义。公开广告产品及功能可用性可能随市场、资格、购买方式和时间改变；以 TikTok for Business、Google Ads、Google Ad Manager 等公开资料为准。

**与策略风控仓库的分工**：[Platform-Strategy-and-Risk-Control](https://github.com/ZeyuYu713/Platform-Strategy-and-Risk-Control) 研究平台策略、内容治理和风险控制；本仓库重点是广告业务与商业产品，交叉部分仅为理解广告生态而保留。

> Independent learning notes. Not an official document of TikTok, ByteDance, Google or other advertising platforms. Revised: 2026-11-12.
