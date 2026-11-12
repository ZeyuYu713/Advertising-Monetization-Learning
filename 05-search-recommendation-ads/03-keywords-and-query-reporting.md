# 03｜Keyword、Search Term、Match Type 与 Impression Share

## 一、一个典型关键词问题

广告主投“running shoes”，用户实际搜索“best running shoes for beginners”。这是 Keyword（投放/定向词）与 Search Term（实际查询词）的区别。

**Exact / Phrase / Broad** 通常对应不同的匹配控制和覆盖广度，具体匹配算法可能考虑近义、语境及平台实现；不能仅根据字面推断所有词都必须完全一致。

**Negative Keywords（否定关键词）**用于排除不适合的查询，但配置过宽可能意外损失相关流量，不能无限堆叠排除词。

TikTok 的关键词管理说明：[How to Find Keywords for Search Ads](https://ads.tiktok.com/resources/help/article/how-to-find-keywords-for-search-ads?lang=en)。

## 二、Search Ads 的对象层级

公开 TikTok Search Ads Campaign 指南说明，关键词在 Ad Group 层面设置；广告组可关联若干 Ad 和 Creative。搜索词报告、关键词报告与 Ad Group 的指标不一定有完全一致的可见数据。

**一个重要的隐私限制**：TikTok 2026 年4月的 [搜索推广系列报告官方说明](https://ads.tiktok.com/resources/help/article/reporting-on-search-ads-campaigns?lang=zh) 指出，搜索词/关键词与创意层级的交叉细分受到披露限制，部分低量转化不会单独展示，且报表存在时延。

因此，**不要为了做“Creative×Search Term×Conversion”的任意下钻而假定平台一定暴露所有数据**。

## 三、Impression Share 和 CTR 不是一个概念

Impression Share（IS）在符合条件的报告里反映捕获的合格搜索展示机会份额，而 CTR=Clicks/Impressions 衡量已经展示之后的点击比例。前者关心覆盖机会，后者关心展示后的用户反应。

教学例子：符合账户产品定义的合格搜索展示机会为 2,000,000 次，广告实际获得 500,000 次，IS=25%。若其中发生 10,000 点击，CTR=2%。

$$
IS=\frac{500{,}000}{2{,}000{,}000}=25\%
$$

$$
CTR=\frac{10{,}000}{500{,}000}=2\%
$$

实际 IS 的“合格机会”由平台产品口径定义，不能把某关键词的全网搜索量直接当分母。

官方：[About Impression Share for Search Ads](https://ads.tiktok.com/resources/help/article/about-the-impression-share-metric-for-search-ads)，2026-09。

## 四、为什么点击增长不等于查询质量提高？

如果新增关键词覆盖不相关搜索，即使展示、点击总量增长，实际购买转化可能下降，且用户搜索体验受损。应同时观察相关性、搜索 CTR、CVR、CPA、有效转化、负反馈和长周期满意度。

## 五、产品能力设计

面向搜索广告客户，可提供关键词推荐、低覆盖原因诊断、否定关键词管理、关键词—创意相关性检查、IS 与预算联动提示，并对低样本/隐私限制清晰说明。

不能以“让所有查询都能被广告购买”作为目标，应考虑用户意图、政策和搜索结果可信度。

## 面试题

**问**：客户说自己“购买了很多关键词，但搜索广告没有曝光”，怎么分析？

**答**：先查产品资格、预算/出价、关键词是否与真实查询有足够匹配量、否定词及受众限制、广告相关性和审核状态；结合 Impression Share（若可用）和报表时延定位，而不是单纯增加关键词数量。

[上一节](02-search-ads-intent.md) · [下一节](04-feed-ranking-and-ad-insertion.md) · [目录](README.md)
