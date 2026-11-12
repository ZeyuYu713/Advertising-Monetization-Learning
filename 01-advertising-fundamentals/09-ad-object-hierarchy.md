# 09｜广告业务对象层级：Account、Campaign、Ad Group、Ad、Creative、Video

**Ad Object Hierarchy & Data Relationships｜广告业务、数据分析与风控策略的共同基础**

> 核心判断：**投放管理层级不等于素材资产层级，更不等于风控处置层级。** 在 TikTok Ads 常规投放中，Campaign → Ad Group → Ad 是官方的主要管理结构；Creative、Video、Image、Post、Landing Page 和 Domain 则要结合实际产品与数据模型确认其关联。升级后的 Smart+ 还有明确的 Creative 报表层级，不能把所有场景强行简化成一棵固定的树。

## 1. 为什么要区分对象层级？

面试或业务分析中，经常会听到“广告计划维度”“Ads 维度”“Creative 维度”“Video 维度”“广告主维度”。如果不先确认每个词指的是什么，就可能在数据统计、风险命中和策略处置时出错。

例如：同一段视频素材被 5 条 Ad 使用。某条视频被风险模型识别一次，究竟是“1 条风险视频”“5 条风险广告”，还是“1 个风险广告主”？三个答案取决于被统计的**对象类型及去重规则**，不能混用。

还要注意：“广告计划”在中文日常沟通里可能指 Campaign，也可能泛指某条投放广告；**Ads/Ad 通常指广告对象 Ad，不应自动翻译成 Campaign**。

## 2. 官方投放管理结构：Campaign → Ad Group → Ad

TikTok Ads Manager 官方的常规结构分三层：

~~~mermaid
flowchart TD
    A["Ad Account / advertiser_id<br/>广告账户（报告口径需确认）"]
    B["Campaign / campaign_id<br/>广告系列：目标、系列预算等"]
    C["Ad Group / adgroup_id<br/>广告组：受众、广告位、排期、优化与出价"]
    D["Ad / ad_id<br/>广告：具体广告设置、文案与素材引用"]
    A --> B --> C --> D
~~~

> 此图反映常规 Ads Manager 的逻辑结构，不代表所有产品、API 或内部数据库都有完全一致的对象层级及字段。部分 Campaign、Ad Group 的预算配置取决于具体投放设置。

| 层级 | 英文 | 通常解决什么问题？ | 常见分析单位 |
| --- | --- | --- | --- |
| 广告账户 | Ad Account / Advertiser Account | 账户管理、权限、支付、可投业务 | account / advertiser_id |
| 广告系列 | Campaign | 整体营销目标与系列层管理 | campaign_id |
| 广告组 | Ad Group | 受众定向、广告位、排期、预算/优化与出价设置 | adgroup_id |
| 广告 | Ad | 一条被配置和投放的广告，关联素材、文案、链接等 | ad_id |

**Advertiser 是个有歧义的词**：商务语境指品牌或购买广告的公司，广告 API 的 **advertiser_id** 通常是账户/报告对象标识。一个商业品牌可能拥有多个广告账户，因此“1 万个 advertiser_id”不必然等于“1 万家独立企业”。

官方依据：[About TikTok Ads Structure](https://ads.tiktok.com/resources/help/article/tiktok-ads-structure)；[How to Create an Ad](https://ads.tiktok.com/resources/help/article/ad-set-up?lang=en)。

## 3. 重要例外：2026 年升级 Smart+ 的四层结构

TikTok 官方的 **Reporting for the upgraded Smart+ experience**（更新于 2026 年 1 月）明确把报告结构写为：

~~~mermaid
flowchart TD
    A["Campaign / Campaign ID"]
    B["Ad Group / Ad Group ID"]
    C["Ad / Ad ID<br/>聚合创意资产"]
    D["Creative / Smart+ Creative ID<br/>单项素材资产：视频、图像、文字等"]
    A --> B --> C --> D
~~~

这说明**Creative 在特定产品里可以是一个具有独立 ID 的报告对象**，不能统一说 Creative 永远只是抽象的创意概念。

| 场景 | 推荐说法 | 不严谨的说法 |
| --- | --- | --- |
| 常规 Ads Manager 投放结构 | Campaign、Ad Group、Ad 三个主要管理层级 | 所有广告平台都只有三层 |
| 升级 Smart+ 报表 | Campaign、Ad Group、Ad、Creative 四层，有 Smart+ creative ID | Creative 必然等于整个 Ad |
| 广告制作/营销语境 | Creative 可指创意概念或完整创意成品 | Creative 在所有场景都仅代表 Video |
| 风控数据语境 | Creative 具体是哪种对象，需看其 ID 与 schema | creative_id 总能直接等同于 video_id |

官方：[Reporting for the upgraded Smart+ experience](https://ads.tiktok.com/resources/help/article/about-reporting-for-the-upgraded-smart-experience)。

**面试要点**：被问“Creative 是什么”，先区分**营销创意概念、广告创意实例、Smart+ Creative 报表对象**。这三种说法在不同团队同时存在，不应互相覆盖。

## 4. 素材与外部内容：更接近“关联图”，不是固定的下一级

广告系统除了投放层级，还可能涉及：

| 对象 | 英文 | 可能的标识 | 与广告的关系 |
| --- | --- | --- | --- |
| 视频素材 | Video Asset / Video Material | video_id、video_material_id 等 | 可能被多个广告或创意引用 |
| 图片素材 | Image Asset | image_id 等 | 可能被多个广告或创意引用 |
| 原生帖子 | Organic Post | post_id | Spark Ads 可利用自有/经授权帖子 |
| 创意文案与 CTA | Ad Text / CTA | 平台具体实体 ID | 可组成或配置创意，具体依产品而定 |
| 落地链接 | Landing URL | URL / tracking link | 广告点击或交互可能通向外部页面 |
| 网站与域名 | Domain / Website | Host、注册域、站点 ID 等 | 多条 URL 可能属于一个域名 |
| 页面内容 | Landing Page / Product / Checkout Page | URL、页面快照、抓取任务 ID | 是风控/转化场景关联对象，不是必然的广告管理子节点 |

示意：

~~~mermaid
flowchart LR
    AD1["Ad A"] --> CR1["Creative X"]
    AD2["Ad B"] --> CR1
    CR1 --> V1["Video V1"]
    CR1 --> T1["Text / CTA"]
    AD1 --> LP1["Landing URL 1"]
    AD2 --> LP2["Landing URL 2"]
    LP1 --> DM["Domain example.test"]
    LP2 --> DM
    POST["Authorized organic post"] -. "Spark Ads 条件下" .-> CR1
~~~

**教学示意，不是官方内部 ERD。** 目的是表达“同一素材可能被复用”“多个 URL 可能指向同一域名”“Spark Ads 可能关联原生帖子”等关系。实际系统可能将 Creative 和 Asset 合并成同一实体，也可能使用不同关联表。

这里尤其要避免三个错误：

**错误 A：Video ID = Ad ID。** 一段视频被多个广告使用时，两者不是一个概念；但具体是否复用、如何分配 ID，须按平台数据模型检查。

**错误 B：Landing Page 属于 Video 子层级。** 落地页通常是广告、链接或投放内容所引用的外部资源，并不是视频的子对象。

**错误 C：命中 Domain 就说明每个引用该域名的 Ad 都确认违规。** 域名风险可能是证据之一，是否对关联广告或账户处置，要看证据、时间、关联强度和策略规则。

Spark Ads 官方说明：[About Spark Ads](https://ads.tiktok.com/resources/help/article/spark-ads?lang=en)，2026 年 6 月更新。

## 5. 同一个“维度”其实可能回答三个不同问题

| 视角 | 关注对象 | 典型问题 |
| --- | --- | --- |
| 投放管理层级 | Campaign / Ad Group / Ad | 预算是谁配置的？哪个投放单元交付不足？ |
| 创意与内容粒度 | Creative / Video / Image / Post / Landing Page | 哪种素材重复使用？识别结果对应哪个内容？ |
| 风险处置层级 | Ad / Account / Domain（按业务规则） | 标记了什么？复核什么？最终限制谁？ |

**识别粒度（Detection Granularity）**是模型判断什么对象；**处置粒度（Enforcement Granularity）**是策略最终作用于什么对象。前者可以是 Video 或页面，后者可以是 Ad 或账户，但绝不意味着“视频一命中就自动封账户”。

## 6. 可复算的完整例子

假设有以下虚构关系：

| Advertiser ID | Campaign | Ad Group | Ad | Creative | Video | Domain |
| --- | --- | --- | --- | --- | --- | --- |
| A01 | C01 | G01 | AD01 | CR01 | V01 | shop-a.test |
| A01 | C01 | G01 | AD02 | CR02 | V01 | shop-a.test |
| A01 | C02 | G02 | AD03 | CR03 | V02 | shop-b.test |
| A02 | C03 | G03 | AD04 | CR04 | V01 | shop-c.test |

假设风险识别只在 V01 产生一次**待核查的模型命中**，则：

- 风险视频素材：**1 个独立 Video（V01）**；
- 如果该素材的关联在当前时间窗内有效，受影响的广告候选：**3 个 Ad（AD01/AD02/AD04）**；
- 关联广告账户：**2 个 Advertiser ID（A01/A02）**；
- 可直接确认违规的广告：**不能由上述命中数字自动得出**。

如果 AD04 早已停投、或者替换了素材，不能不看时间就把它算作当前有效的风险投放。

## 7. 对商业产品与广告风控有什么意义？

品牌广告产品团队经常需要 **Ad / Creative / Video** 级别的效果报表，分析广告创意对 Reach、Frequency 和品牌效果的影响。

广告风控团队还需结合落地页、域名、账户，回答“风险证据发生在哪里”“影响哪些真实投放对象”“能否避免重复扫描”“如何防止风险从内容级误扩散到账户级”。

同一个风控 Video 结果如果被 5 条 Ad 复用，那么若计算模型准确率用素材为单位、计算风险广告覆盖量用 Ad 为单位，就必须向业务说明分母与关联规则不同。

## 8. 面试回答框架（60 秒）

> 我会先把广告系统分成**投放管理层级**和**素材及风险对象层级**。常规 TikTok Ads Manager 主要是 Campaign、Ad Group、Ad 三层，广告账户在上面提供管理范围；但是 Creative、Video、Image、Landing Page 不应全部机械理解为 Ad 下一层的树状关系，素材可能被多条广告复用，落地页也可能与多个广告关联。升级后的 Smart+ 报表还把 Creative 明确作为第四个报告层级。因此做数据分析或风险识别时，我会先确认对象 ID 和基本粒度，再决定去重、关系映射与结果统计口径。比如一段视频命中风险，不等于所有关联广告和广告主都已经确认违规，更不能直接升级成账户处置。

这是一套**基于公开产品结构的分析框架**，不是对任何未公开内部系统结构的披露。

## 9. 官方参考与适用边界

- [TikTok Ads Structure](https://ads.tiktok.com/resources/help/article/tiktok-ads-structure)：常规 Campaign → Ad Group → Ad。
- [How to Create an Ad](https://ads.tiktok.com/resources/help/article/ad-set-up?lang=en)：Ad 中可涉及图片、视频、文案与跟踪。
- [Upgraded Smart+ reporting](https://ads.tiktok.com/resources/help/article/about-reporting-for-the-upgraded-smart-experience)：四级 Campaign / Ad Group / Ad / Creative，2026 年 1 月更新。
- [TikTok Business API：Supported dimensions](https://business-api.tiktok.com/gateway/docs/index?doc_id=1751443956638721)：部分报告包括 advertiser_id、campaign_id、adgroup_id、ad_id、video_material_id、post_id 等维度；**不同报告支持不同组合**。
- [Spark Ads 官方说明](https://ads.tiktok.com/resources/help/article/spark-ads?lang=en)：与原生帖子和授权关联。

**资料核对日期：2026-11-12。** TikTok 不同产品、账户和 API 版本的对象模型可能不同；本篇不包含字节内部表结构和非公开识别规则。

[下一篇：对象粒度、SQL 统计与种子样本](10-object-granularity-and-risk-sampling.md) · [返回第一章](README.md)
