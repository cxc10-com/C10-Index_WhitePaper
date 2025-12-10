# C10 Index 加权加密货币指数 — 白皮书（En）

**Date**：2025-10-18
**Sponsor**：CXC10
**Base Value**：1,000（Base Date：2025-08-16 UTC 00:00）
**Base Currency**：US Dollar（USD）
**Data Source**：CoinGecko

## 1. Overview

C10 Index 加权加密货币指数（以下简称“指数”）衡量按**自由流通市值**排名前十的加密资产（不含稳定币、包装代币和质押凭证）的整体表现，并通过**修正市值加权**限制单一资产集中度。指数设定**单一成分权重上限为 50%**，适用于基准衡量、投资组合构建及相关产品复制。

## 2. Index Summary

* **Universe**：Cryptoassets that meet the eligibility criteria and are freely tradable on qualified spot exchanges
* **Number of Constituents**：10
* **Index Currency**：USD
* **Weighting Method**：Free-float market-cap weighting with a 50% cap; excess weight is redistributed iteratively on a pro-rata basis
* **Constituent Review (Reconstitution)**：Quarterly
* **Weight Review (Rebalancing)**：Monthly
* **Publication Frequency**：Real-time，**updated every minute**；daily close fixed at UTC 00:00
* **Corporate Actions and Special Events**：Forks, airdrops, burns, unlocks and similar events are handled via**divisor**or**constituent**adjustments

## 3. Inclusion and Exclusion Criteria

### 3.1 Inclusion Criteria

1. Listed on at least two qualified exchanges with transparent and auditable market data.
2. 可获得 USD 计价价格（直接或通过主流 USD 稳定币对）
3. Liquidity: the median daily trading volume over the past 90 calendar days is at least USD 10 million, with at least 85 days of valid (non-zero) trading
4. Free-float supply data can be independently verified
5. 在初始及持续基础上，该基础商品在属于 Intermarket Surveillance Group（“ISG”）成员的市场上交易，且指数所对应的挂牌交易所能够从该 ISG 成员处获取与该商品相关的交易及监管信息；
6. 在初始及持续基础上，该基础商品作为期货合约的标的资产，且该期货合约已在指定合约市场（Designated Contract Market，“DCM”）连续上市交易至少六个月，同时指数所对应的挂牌交易所与该 DCM 之间存在全面的监管信息共享安排（包括直接签署的监管共享协议，或通过双方同为 ISG 成员实现的信息共享）；
7. 在初始基础上，存在一只交易型开放式指数基金（ETF）已在全国性证券交易所上市交易，且该 ETF 对该基础商品的经济敞口不低于其净资产价值的 40%。

### 3.2 Exclusion Rules

* Stablecoins（USDT、USDC、USDE etc）
* Wrapped or cross-chain representations of other assets（WBTC、WETH、stETH、wstETH、eETH、WBETH etc）
* Assets subject to material regulatory, legal, or security risks (at the discretion of the Index Committee)
* Multiple tickers representing the same underlying economic exposure, in which case only the primary representation is retained

## 4. Constituent Selection, Reconstitution, and Rebalancing

### 4.1 Quarterly Reconstitution

On the second-to-last business day of each calendar quarter (the “Selection Date”), the Index universe is sorted by**free-float market capitalization**（USD）, and the top ten assets are selected as constituents. The data definition follows Section 5.Changes are announced at least three business days in advance, and the new constituents become effective at 00:00 UTC on the first day of the next quarter.

### 4.2 Monthly Rebalancing

On the first day of each calendar month at 00:00 UTC, the Index**locks in constituent shares**in accordance with the calculation methodology. No changes to the set of constituents are made during monthly rebalancing; only weights (via locked shares) are adjusted.

### 4.3 Non-Regular Adjustments

In the event of delisting, prolonged trading suspension, or major security incidents, the Index may implement ad-hoc constituent substitutions and divisor adjustments to preserve index continuity.

## 5. Price and Supply Conventions

### 5.1 Price

* Prices are derived from qualified spot exchanges quoting against USD (including major USD stablecoin pairs), using a**volume-weighted median price**or **VWAP (1-minute window)**
* Outliers are removed using robust statistical methods

### 5.2 Free-Float Supply

* Free-float supply is obtained from reputable data sources (primarily CoinGecko), with adjustments for locked tokens (team, foundation holdings), known lost coins, duplicate representations, etc
* All adjustments must be logged and recorded for auditability

### 5.3 Market Capitalization

The free-float market capitalization at time $t$ is：

$$
\text{MarketCap}_{i,t} = Q_{i,t}\cdot P_{i,t}
$$

* $Q_{i,t}$：**Free-float supply** of asset $i$ at time $t$ 
* $P_{i,t}$：USD price of asset $i$ at time $t$

### 5.4 Index Definition

Within a rebalancing interval $[r, r^+)$, the Index level is computed using**locked shares**and the **divisor**：

$$
I_t = \frac{\sum_{i\in C} AS_i^{(r,nat)}\cdot P_{i,t}}{D}
$$

* $C$：Constituent set at the current rebalancing
* $D$：Divisor applicable from the rebalancing time
* $AS_i^{(r,nat)}$：**Snapshot of the natural free-float number of shares** of asset $i$ at the rebalancing base time $r$, defined and locked as：

$$
AS_i^{(r,nat)} \equiv Q_{i,r},
\qquad t\in[r, r_{\text{next}}) \text{ during which it remains constant。}
$$

### 5.5 Initial Calibration of the Divisor $D$ 

On the base date $t_0$, the Index level is set to $I_{t_0}=1000$：

$$
D_{\text{initial}}
=\frac{\sum_{i\in C_{t_0}} AS_i^{(t_0,nat)}\cdot P_{i,t_0}}{1000}
=\frac{\sum_{i\in C_{t_0}} AS_i^{(t_0,nat)}\cdot P_{i,t_0}}{I_{t_0}}
$$

### 5.6 Divisor $D$ Adjustment at Rebalancing

To ensure that the Index**does not jump**at the rebalancing point, define:

$$
I_r \coloneqq I_{r^-}, \qquad P_{i,r}\coloneqq P_{i,r^-}
$$

- $I_r$：Index value immediately after the rebalancing time
- $I_{r-}$：Index value immediately before the rebalancing time
- $P_{i,r}$：USD price of asset $i$ immediately after the rebalancing time
- $P_{i,r-}$：USD price of asset $i$ immediately before the rebalancing time

Thus:

$$
I_{r^-}=\frac{\sum_{i\in C_{\text{old}}} AS_i^{(r^-,nat)}P_{i,r}}{D_{\text{old}}},\qquad
I_{r}=\frac{\sum_{i\in C_{\text{new}}} AS_i^{(r,nat)}P_{i,r}}{D_{\text{new}}}
$$

- $AS_i^{(r,nat)}$：Snapshot of the natural free-float shares of asset $i$ at the current rebalancing base
- $AS_i^{(r-,nat)}$：Snapshot of the natural free-float shares of asset $i$ at the previous rebalancing base

Setting $I_r=I_{r^-}$ yields the familiar “ratio method”：

$$
D_{\text{new}}
= D_{\text{old}}\cdot
\frac{\sum_{i\in C_{\text{new}}} AS_i^{(r,nat)}P_{i,r}}
{\sum_{i\in C_{\text{old}}} AS_i^{(r^-,nat)}P_{i,r}}
$$

Alternatively, directly from the definition：

$$
D_{\text{new}}
=\frac{\sum_{i\in C_{\text{new}}} AS_i^{(r,nat)}P_{i,r}}{I_r}
$$

- $D_{\text{old}}$：Divisor prior to rebalancing
- $D_{\text{new}}$：Divisor after rebalancing
- $C_{\text{old}}$：Constituent set prior to rebalancing
- $C_{\text{new}}$：Constituent set after rebalancing

### 5.7 Index Calculation Methodology

For any rebalancing time $r$ , the divisor satisfies:

$$
D=\frac{\sum_{i\in C} AS_i^{(r,nat)}P_{i,r}}{I_r},
$$

从而 5.5 的初始定标（with $r=t_0$）与 5.6 的再平衡接平在同一表达式下统一。

Substituting into the Index formula:

$$
I_t=\frac{\sum_{i\in C} AS_i^{(r,nat)}\cdot P_{i,t}}{D}
$$

we obtain:

$$
I_t
= \frac{\sum_{i\in C} AS_i^{(r,nat)} P_{i,t}}
{\dfrac{\sum_{j\in C} AS_j^{(r,nat)} P_{j,r}}{I_r}}\
= I_r \cdot
\frac{\sum_{i\in C} AS_i^{(r,nat)} P_{i,t}}
{\sum_{j\in C} AS_j^{(r,nat)} P_{j,r}}\
= I_r \cdot
\sum_{i\in C}\frac{AS_i^{(r,nat)} P_{i,t}}{\sum_{j\in C} AS_j^{(r,nat)} P_{j,r}}
$$

对分子逐项同时除以基期价格 $P_{i,r}$：

$$
AS_i^{(r,nat)} P_{i,t}
= AS_i^{(r,nat)}\left(P_{i,r}\cdot \frac{P_{i,t}}{P_{i,r}}\right)\
= \underbrace{\big(AS_i^{(r,nat)} P_{i,r}\big)}_{\text{基期的流通市值}}
\cdot
\underbrace{\frac{P_{i,t}}{P_{i,r}}}_{\text{价格收益}}
$$

Hence:

$$
I_t
= I_r \cdot
\sum_{i\in C}\frac{\left(AS_i^{(r,nat)} P_{i,r}\right)\cdot \frac{P_{i,t}}{P_{i,r}}}{\sum_{j\in C} AS_j^{(r,nat)} P_{j,r}}\
= I_r \cdot \sum_{i\in C}
\underbrace{\frac{AS_i^{(r,nat)}P_{i,r}}{\sum_{j\in C} AS_j^{(r,nat)}P_{j,r}}}_{w_i^{(r,nat)}}
\cdot
\underbrace{\frac{P_{i,t}}{P_{i,r}}}_{\text{价格收益}}
$$

Let

$$
w_i^{(r,nat)}=\frac{AS_i^{(r,nat)}P_{i,r}}{\sum_{j\in C}AS_j^{(r,nat)}P_{j,r}},
\qquad \sum_{i\in C} w_i^{(r,nat)}=1
$$

then:

$$
I_t = I_r \cdot \sum_{i\in C} w_i^{(r,nat)} \cdot \frac{P_{i,t}}{P_{i,r}}
$$

> 解释：指数等于**基期点位**乘以**加权价格变化**；此处 $w_i^{(r,nat)}$ 为再平衡基点的**自然权重**。

## 6. Weighting and Constraints

采用 **自由流通市值加权(Free-Float MCAP)** 并施加约束。

### 6.1 Notation and Setup

* $C$：本次再平衡的成分集合； $r$：再平衡生效时刻； $t\in[r, r_{\text{next}})$。
* $Q_{i,r}$：成分 $i$ 在 $r$ 的**自由流通供应量快照**。
* $P_{i,r}$、 $P_{i,t}$：成分 $i$ 的价格。
* $AS_i^{(r,nat)}$：资产 $i$ 在本次再平衡基点的自然自由流通供应量快照
* $AS_i^{(r,cap)}$：资产 $i$ 在本次再平衡基点的约束自由流通供应量快照
* $I_r$：指数在 $r$ 的点位； $D$：对应除数。

### 6.2 Natural Weights  $w_i^{\mathrm{nat}}$

Based on the snapshot of unconstrained natural free-float shares（ $AS_i^{(r,nat)}\equiv Q_{i,r}$ ）the natural weight is：

$$
w_i^{(r,nat)}=\frac{AS_i^{(r,nat)}P_{i,r}}{\sum_{j\in C}AS_j^{(r,nat)}P_{j,r}},
\qquad \sum_{i\in C} w_i^{(r,nat)}=1
$$

### 6.3 Applying the 50% Cap and Iterative Redistribution to Obtain Constrained Weights $w_i^{\mathrm{cap}}$

对自然权重施加单一资产上限 50%，并将超额按未触顶成分的自然权重比例分配，必要时迭代直至全部 $\le 50$%。

Define the capped set $O={i\mid w_i^{\mathrm{nat}}\ge 50}$% and the uncapped set $U=C\setminus O$。

Define the total excess：  

$$
E=\sum_{i\in O}\bigl(w_i^{\mathrm{nat}}-50％\bigr)
$$

Then the constrained weights are：

$$
w_i^{\mathrm{cap}} =
\begin{cases}
50％,& i \in O,\\
w_i^{\mathrm{nat}} +
\dfrac{w_i^{\mathrm{nat}}}{\displaystyle\sum_{k\in U} w_k^{\mathrm{nat}}}E, & i \in U.
\end{cases}
$$
  
If any newly computed $w_i^{\mathrm{cap}}>50\%$，$O,U,E$ are updated and the procedure is repeated until convergence. Small numerical discrepancies may be corrected by a final normalization such that $\sum_i w_i^{\mathrm{cap}}=1$。

### 6.4 Normalization to Ensure $\sum_i w_i^{\mathrm{cap}}=1$

经过6.3节计算，得到一组暂态约束权重 $\widehat{w}_i$，其中
- 对触顶集合 $O$： $\widehat{w}_i\le 50\%$ 且通常 $\widehat{w}_i=50$%；
- 对未触顶集合 $U$： $\widehat{w}_i<50$%。

由于数值误差， $\sum_i \widehat{w}_i$ 可能不等于 1。为既精确合计为 1又不破坏上限，只对U集合做等比缩放、保持O集合不变。

给定

$$
\sum_i w_i^{\text{cap}} = S_O + \alpha S_U = 1
$$

 其中

$$  
S_O = \sum_{i\in O} \widehat{w}_i,\qquad  
S_U = \sum_{i\in U} \widehat{w}_i
$$

得到

$$
\alpha = \frac{1 - S_O}{S_U}
$$

所以

$$
w_i^{\mathrm{cap}} =
\begin{cases}
50％, & i \in O,\\
\alpha\widehat{w}_i, & i \in U.
\end{cases}
$$

### 6.5 关于 $AS_i^{(nat)}$修正为 $AS_i^{(cap)}$

为引入50%单一资产上限并在基点 $r$ 精确复制该约束后的约束权重，我们在 $r$ 时对 $AS_i^{(nat)}$作一次性更新为 $AS_i^{(cap)}$；其后份额在整个区间 $[r, r_{\text{next}})$ 保持不变，指数仅随价格波动。 

$$
AS_i^{(r,cap)} =
\begin{cases}
AS_i^{(r,nat)}\times
\underbrace{\dfrac{w_i^{(r,cap)}}{w_i^{(r,nat)}}}_{\gamma_i\;\text{（权重修正系数）}}, & w_i^{(r,nat)}>0,\\
0, & w_i^{(r,nat)}=0.
\end{cases}
$$

### 6.6 修正后的指数计算公式

$$
I_t = I_r \cdot \sum_{i\in C} w_i^{(r,cap)} \cdot \frac{P_{i,t}}{P_{i,r}}
$$

## 7. 发布频率

* 实时发布，**每分钟刷新**
* 每日收盘取 UTC 00:00

## 8. 加密资产“公司行为”处理

* **硬分叉**：视为分拆，调整除数，新链不自动纳入，下次重构评估
* **空投**：视为特殊分红，调整除数
* **合并/兑换/改名**：保持经济等价调整，必要时调除数
* **销毁/增发/解锁**：反映在供应量口径（影响未来权重），**不调除数**
* **退市/暂停**：无可靠价格可替换并调整除数

## 9. 治理

* **指数委员会**：维护方法论、合格交易所名单，裁定异常
* **方法论变更**：重大调整提前 ≥ 5 个工作日公告（紧急除外）
* **数据政策**：所有来源与调整留痕备查
* **年度审查**：参数与规则年度复核

## 10. 发布与授权

* **Codes and Identifiers**：Index code: C10; additional identifiers may be obtained as needed
* **Licensing**：Use of the Index for financial products requires a license agreement
* **Dissemination**：The Index is disseminated via administrator APIs/data feeds and the official website

## 11. 免责声明

本文档仅供参考，不构成投资建议或买卖要约。加密资产波动性高，可能损失全部价值。管理员力求准确，但不保证结果，方法论可按治理政策调整。历史表现不代表未来结果。
