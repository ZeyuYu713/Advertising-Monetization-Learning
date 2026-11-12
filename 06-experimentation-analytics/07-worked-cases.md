# 07｜广告实验、归因与分析：10道计算和产品题

全部为虚构教学案例。每题必须先确认单位、分母与实验假设，再解释结果。

## 题1：五个广告指标

曝光100万、点击2万、转化500、花费$10,000。

CTR=2%；CPM=$10；CPC=$0.50；点击后CVR=2.5%；CPA=$20。转化使用本题一致的统计窗口。

## 题2：百分点和相对提升

A组转化率20%、B组22%。绝对增加2个百分点，相对提升10%；不能说“提高2%”而不指明语义。

## 题3：95%近似置信区间

各组1万用户，A2000转化、B2200转化。独立二项近似标准误0.00576；差异0.02±1.96×0.00576，即约[0.87,3.13]个百分点。假设满足时支持正向差异。

## 题4：SRM

原计划50:50分流，实际12,000:8,000，总20,000。期望各10,000，卡方统计量800，自由度1，严重偏离。应先排查分流、资格过滤、埋点和去重，不能直接报告业务结果。

## 题5：归因 vs 增量

广告主报表有1000次归因购买。另有随机研究，处理组10万人中3000购买，对照组10万人中2800购买。若实验设计及标签记录可信，估计绝对增量为每10万人约200购买。不能把1000次归因购买直接当1000次增量。

## 题6：Creative 联表

Ad表100条，累计花费$5000；每条 Ad 映射3条素材。若错误地直接JOIN后SUM(ad级花费)，可能得到$15,000，真实总花费并未改变。

## 题7：净增量广告收入

新品牌广告位报表收入$200,000，旧产品预算迁移$130,000，新增成本$30,000。在极简且无其他影响的假设下，新增收入$70,000，新增贡献$40,000。

## 题8：样本量

基线转化率20%，MDE绝对2个百分点、α=0.05（双侧）、功效80%，独立等量两组的简化近似每组需6272人；MDE减半，样本量约变成4倍。实际要根据方差和随机化单位修正。

## 题9：品牌广告报告产品

用户反馈第三方可见率与平台曝光不一致。先核对普通曝光/可见曝光的指标定义、过滤、报告窗口、设备、数据延迟和第三方适用范围；再设计指标字典、差异说明、对账流程与异常诊断。不能未经调查称任何一方造假。

## 题10：英文面试

**Q: What is the difference between attributed and incremental conversions?**

**A:** Attributed conversions are outcomes credited to an advertising touchpoint under a selected attribution rule. Incremental conversions are the additional outcomes caused by advertising compared with a credible counterfactual. Attribution and causality are different questions.

[上一节](06-experiment-risks.md) · [返回目录](README.md) · [下一章](../07-product-cases-interviews/README.md)
