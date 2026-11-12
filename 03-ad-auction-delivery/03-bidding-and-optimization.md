# 03｜出价策略、优化目标与计费事件：三个容易混淆的维度

## 一、Objective、Optimization、Billing 与 Bid 的区别

| 概念 | 回答什么问题？ | 示例 |
| --- | --- | --- |
| Objective | 客户最终想达到什么 | Reach、Video Views、Sales |
| Optimization Goal | 系统优先寻找什么目标行为 | 展示、点击、转化（按产品开放） |
| Billing Event | 客户依什么事件计费 | Impression、Click 或合同约定 |
| Bidding Strategy | 如何控制投放成本与规模 | Maximum Delivery、Cost Cap 等 |
| Bid / Cost Control | 客户愿意承担的价格或目标 | 目标 CPA、CPC、CPM 等 |

**oCPM（optimized CPM）**不应仅凭名称理解为“按转化计费”。它通常指以曝光计费框架服务于转化等优化目标的机制；实际产品目标、计费方式和展示口径以官方配置为准。

## 二、为什么最低 CPA 不一定意味着最多转化？

假设两条广告方案均已达相同质量要求：

| 方案 | 花费 | 转化数 | CPA |
| --- | ---: | ---: | ---: |
| A | $5,000 | 100 | $50 |
| B | $8,000 | 200 | $40 |

B 的 CPA 更低、转化数更高，但如果客户预算上限只有 $6,000，不能直接按 B 全规模执行。还需检查库存可用性、转化质量与真实增量，以及成本随规模变化是否稳定。

进一步看转化价值：若 A 的每次转化平均带来 $120 可归因收入，B 的每次转化仅 $60，A 的归因 ROAS 为 2.4，B 为 1.5。可归因收入也不等于利润或因果增量，需明确目标。

## 三、Maximum Delivery 与 Cost Cap

TikTok 2025 年公开的 [Bidding Interface](https://ads.tiktok.com/help/article/bidding-interface?lang=en) 说明在适用目标下支持 Maximum Delivery 与 Cost Cap 等竞价策略。简单理解：

**Maximum Delivery**：在预算与其他约束下争取更多目标结果，实际单位成本可能波动。

**Cost Cap**：以目标成本控制为约束优化投放，并非在每次结果上都严格不超过该值；若限制过紧，可能出现交付不足。

产品和账户的具体名称、资格、目标适用性会更新，不能把上述定义泛化为全部广告类型。

## 四、为什么 Cost Cap 过低会低消耗？

在教学模型里，一次广告机会的预估结果成本若高于客户设定的容忍范围，就可能无法以预期经济价值参与竞争。若所有合格机会成本都更高，预算可能花不出去。

**面试回答**：不要直接把所有低消耗归因于 Cost Cap。先排查计划状态、定向、素材审核、学习阶段、市场竞争、目标事件回传和预算，再检验 Cost Cap 的可执行性。

## 五、学习阶段为什么与转化信号相关？

当广告系统需要估计“哪些用户更可能产生某个行为”，足够、可靠且及时的结果反馈对估计质量至关重要。若转化事件发生率低、回传延迟或上报错误，优化容易出现不稳定。

但具体“达到多少次转化”“学习周期多长”等是平台、目标和时间相关的公开产品规则，不应当成永恒行业常数。可以查看 [TikTok Auction Delivery Troubleshooting](https://ads.tiktok.com/resources/help/article/troubleshooting-auction-ad-delivery-solutions)。

## 六、业务题：预算花得慢，应该放松什么？

先明确是否为 Auction，若是竞价，按**可投资格、受众规模、素材、出价约束、实际竞争、优化信号、系统交付**逐项定位。对每个可能改动预估其成本、效果和质量护栏；通过适当实验验证，而不是同时大改受众、创意和出价导致无法归因。

[上一节](02-auction-ranking-and-pricing.md) · [下一节](04-pacing-and-learning.md) · [目录](README.md)
