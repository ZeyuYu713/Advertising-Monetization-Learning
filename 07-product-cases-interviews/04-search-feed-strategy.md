# 案例04｜搜索广告与推荐流策略：业务问题如何拆解？

## 题目 A：搜索广告 Impression Share 很低

某搜索广告客户认为覆盖不够，Impression Share=20%，CTR=3%，预算消耗仅50%。

**先统一口径**：IS 分母由平台定义的“符合条件的展示机会”决定，不能用全网搜索总量直接计算；CTR 与 IS 不是一个指标。

**候选原因**：关键词覆盖、匹配方式与否定词、广告/落地页相关性、出价/预算、市场库存、产品资格和系统交付。因实际仅消耗50%，先判断是竞争力不足、可匹配查询少还是异常，而非直接“增加预算”。

**产品动作**：设计搜索词与关键词差异诊断、IS/预算关联提示、低量隐私数据说明与关键词建议。不能承诺一定让所有搜索都有广告。

## 题目 B：自然搜索结果点击率下降，但广告收入上涨

先核查整体搜索需求、自然结果排序、广告负载、位置变化、广告标识与查询质量。自然结果点击下降可能是结果本身更差、广告挤占、搜索词结构变化，也可能用户无需点击便获得答案。

要监测用户满意度、查询后返回搜索比例、自然结果有效消费、广告负反馈及长期留存，采用适当实验识别广告增量与搜索体验的权衡。

## 题目 C：在推荐信息流多插入一条广告

假设每用户每日广告曝光从5增至6，用户数100万，平均 eCPM=$10 且其他变量都固定，静态收入由：

$$
R_0=1000000\times5/1000\times10=\$50{,}000
$$

变为$60,000。但真实用户消费时长、广告定价、库存匹配和长期留存不会保持恒定，必须以实验检验；**这个静态计算不是上线增收预测。**

## 题目 D：推荐的 CTR 很好，但用户越来越疲劳

检查广告/自然内容多样性、同素材重复频次、短期点击与长期满意度差异、负反馈及目标群体变化。不能只优化单次 CTR，要考虑广告主效果和用户体验长期价值。

## 面试英文追问

**Q: How would you balance ad monetization and user experience in a recommendation feed?**

**A:** I would define the incremental revenue objective and user-experience guardrails, identify eligible ad opportunities, and test controlled changes to ad load or relevance. I would not scale a change based solely on short-term impressions or eCPM.

[上一题](03-new-ad-product-prd.md) · [下一题](05-experiment-measurement-cases.md) · [目录](README.md)
