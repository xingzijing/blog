---
title: WorldQuant Alpha中性策略的个股权重
author: zijing
date: 2025-03-06
categories: [Study Notes]
tags: [Quant Fin]
image: /assets/img/post/20241128/blog1-test.png
---

WorldQuant Brain 平台 Alpha 中性策略，此例中 Alpha Value 公式为 $ rank(-return) $, 即基于股价均值回归假设，卖出超买和买入超卖。设置 $ Decay = 1 $, 即 T 期观测，T+1 期交易。

计算个股权重：

- 分位排序归一化 (Rank Normalization)：将原始的收益率数据转换为相对排名，消除不同股票之间收益率绝对值差异的影响，只关心股票的相对优势或劣势。
- 中心化 (去均值化 - Mean Centering)：消除数据的系统性偏差，多空平衡。
- L1 归一化 (L1 Normalization)：将权重的绝对值之和缩放到 1，控制组合的总体风险敞口，确保组合的权重不会过度放大或缩小。

在 Alpha 中性策略中，其他归一化方法：

- Z-score 归一化 (Standardization)：将数据缩放到均值为 0，标准差为 1 的分布，可以消除数据的系统性偏差，考虑数据的波动性。
- Min-Max 归一化 (Min-Max Scaling)：将数据缩放到一个特定的范围如 [0, 1] 或 [-1, 1]，可以保留数据的原始分布，但对异常值敏感。
- L2 归一化 (L2 Normalization)：将权重的平方和缩放到 1。L2 归一化也可以控制组合的风险，但它对大权重的惩罚更重。

引入三期衰减，衰减可防止 alpha 因子过度反应灵敏，减少交易成本和换手率。线性衰减：

$$
Decay\_linear(x,n) = \frac{x[date] \times n + x[date-1] \times (n-1) + \cdots + x[date-n+1] \times 1}{n + (n-1) + \cdots + 1}
$$

![3 table.png](https://api.worldquantbrain.com/content/images/21_y9-wzhl0hc08V2_q6iBlIGOg=/249/original/3_table.png)

Source: [How BRAIN platform works](https://platform.worldquantbrain.com/learn/documentation/create-alphas/how-brain-platform-works)
