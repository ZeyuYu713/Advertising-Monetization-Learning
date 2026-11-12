# 10｜广告经济学速查：口径、模型与资料

## 常用术语

| 英文 | 中文 | 使用提醒 |
| --- | --- | --- |
| Gross Spend | 客户总投放花费 | 不等于平台净收入 |
| Net Revenue | 净收入 | 取决于财务确认口径 |
| Take Rate | 平台收入占交易额比例 | 分子与分母需定义 |
| Inventory | 广告库存 | 预测、可售、已预订、已交付要区分 |
| Ad Request | 广告请求 | 不等于有效曝光 |
| Match / Fill | 请求匹配/填充 | 指标在不同产品有不同分母 |
| Sell-through | 预订库存占预计可售库存 | 不等于实时 Fill |
| Delivery Rate | 同口径交付完成率 | 预测值不一定是承诺 |
| Yield / eCPM | 每千曝光等效收益 | 收入与曝光分子分母一致 |
| Share of Wallet | 客户预算份额 | 需明确可比的媒体预算 |
| Pacing | 花费和交付的时间节奏 | 不一定线性 |
| Cannibalization | 对原产品的预算替代 | 新品收入≠净新增收入 |
| Marginal Return | 边际回报 | 不同预算段回报可能下降 |
| Ad Load | 广告负载 | 没有统一分母 |
| Guardrail | 护栏指标 | 与主目标共同决定上线 |

## 常用公式

同口径等效广告收入：

$$
R=\frac{I}{1000}p
$$

全平台分业务线合计：

$$
R=\sum_i\frac{I_i}{1000}p_i
$$

广告主预算分配教学式：

$$
S=M\times w\times e
$$

对称量价桥接：

$$
\Delta R_I=\frac{I_1-I_0}{1000}\frac{p_0+p_1}{2},\quad
\Delta R_p=\frac{p_1-p_0}{1000}\frac{I_0+I_1}{2}
$$

人均未来贡献简化式：

$$
V(a)=R_{\mathrm{now}}(a)+\delta E[R_{\mathrm{future}}(a)]-C_{\mathrm{experience}}(a)
$$

最后一个是研究框架，不是任何公司的生产算法。

## 五道易混概念题

**问：一个客户批了100万美元预算，平台收入一定有100万美元吗？** 不一定：媒体预算比例、执行情况、交易参与者、会计口径均会改变结果。

**问：eCPM 上涨说明竞价更激烈吗？** 不一定：可能是高价值广告产品占比增加，或曝光结构变化。

**问：Fill Rate 低就是平台流量少吗？** 不一定：可能是需求不足、定向、审核、返回失败与报表定义。

**问：品牌预订能保证预测 Reach 吗？** 要核对合同和产品交付规则，预测不必然等于保证值。

**问：新广告产品收入增长是不是净增量？** 不一定，须核算对既有产品的预算迁移与成本。

## 可核对的公开资料

- [TikTok Auction Ads](https://ads.tiktok.com/resources/help/article/about-auction-ads?lang=en)
- [TikTok Reach & Frequency](https://ads.tiktok.com/help/article/reach-frequency-ad-buying-type?lang=en)
- [TikTok Ads Split Testing](https://ads.tiktok.com/resources/help/article/split-testing)
- [Google Ad Manager 指标定义](https://support.google.com/admanager/table/7568664?hl=en)
- [Google Ad Manager eCPM 指标](https://support.google.com/admanager/answer/6334268)
- [IAB Tech Lab ads.txt](https://iabtechlab.com/ads-txt/)

以上资料用于核对公开产品与术语；案例、定价、算法公式均为教学抽象。更新检查：2026-11-12。

[上一节：案例](09-worked-cases.md) · [返回目录](README.md) · [下一章：竞价与投放](../03-ad-auction-delivery/README.md)
