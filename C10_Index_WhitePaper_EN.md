# C10 Index Free-Float Market-Cap Weighted Cryptocurrency Index — Whitepaper

**Date**：2025-10-18
**Sponsor**：CXC10
**Base Value**：1,000（Base Date：2025-08-16 UTC 00:00）
**Base Currency**：US Dollar（USD）
**Data Source**：CoinGecko

## 1. Overview

The C10 Index **Free-Float Market-Cap** Weighted Cryptocurrency Index (the “Index”) measures the performance of the top ten cryptoassets by free-float market capitalization (excluding stablecoins, wrapped tokens, and staking receipts). The Index employs **a modified market capitalization weighting** scheme to limit concentration in any single asset. **A maximum single-constituent weight of 50% is imposed**, making the Index suitable for benchmarking, portfolio construction, and index-linked products.

## 2. Index Summary

* **Universe**：Cryptoassets that meet the eligibility criteria and are freely tradable on qualified spot exchanges
* **Number of Constituents**：10
* **Index Currency**：USD
* **Weighting Method**：Free-float market-cap weighting with a 50% cap; excess weight is redistributed iteratively on a pro-rata basis
* **Constituent Review (Reconstitution)**：Quarterly
* **Weight Review (Rebalancing)**：Monthly
* **Publication Frequency**：Real-time，**updated every minute** ；daily close fixed at UTC 00:00
* **Corporate Actions and Special Events**：Forks, airdrops, burns, unlocks and similar events are handled via**divisor**or**constituent**adjustments

## 3. Inclusion and Exclusion Criteria

### 3.1 Inclusion Criteria

* Listed on at least two qualified exchanges with transparent and auditable market data
* A reliable USD-denominated price is available, either directly or via a major USD stablecoin trading pair
* Liquidity: the median daily trading volume over the past 90 calendar days is at least USD 10 million, with at least 85 days of valid (non-zero) trading
* Free-float supply data can be independently verified
* On an initial and continuing basis, the underlying commodity (cryptoasset) trades on a market that is a member of the Intermarket Surveillance Group (“ISG”), and the exchange on which an Index-linked product is listed can obtain relevant trading and surveillance information on such commodity from that ISG member
* On an initial and continuing basis, the underlying commodity is the reference asset of a futures contract that has been available to trade on a Designated Contract Market (“DCM”) for at least six months, and the exchange on which an Index-linked product is listed maintains a comprehensive surveillance-sharing arrangement with such DCM (including a direct surveillance-sharing agreement or an arrangement achieved via both parties being ISG members)
* On an initial basis, there exists an exchange-traded fund (“ETF”) listed and traded on a national securities exchange that maintains economic exposure of no less than 40% of its net asset value to such underlying commodity

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
\qquad t\in[r, r_{\text{next}}) \text{ during which it remains constant}
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

For any rebalancing time $r$, the divisor satisfies:

$$
D=\frac{\sum_{i\in C} AS_i^{(r,nat)}P_{i,r}}{I_r},
$$

which unifies the initial calibration in Section 5.5（with $r=t_0$）and the rebalancing continuity condition in Section 5.6 under a single expression

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

Dividing each term in the numerator by the base price $P_{i,r}$：

$$
AS_i^{(r,nat)} P_{i,t}
= AS_i^{(r,nat)}\left(P_{i,r}\cdot \frac{P_{i,t}}{P_{i,r}}\right)\
= \underbrace{\big(AS_i^{(r,nat)} P_{i,r}\big)}_{\text{Base-period free-float market cap}}
\cdot
\underbrace{\frac{P_{i,t}}{P_{i,r}}}_{\text{Price return}}
$$

Hence:

$$
I_t
= I_r \cdot
\sum_{i\in C}\frac{\left(AS_i^{(r,nat)} P_{i,r}\right)\cdot \frac{P_{i,t}}{P_{i,r}}}{\sum_{j\in C} AS_j^{(r,nat)} P_{j,r}}\
= I_r \cdot \sum_{i\in C}
\underbrace{\frac{AS_i^{(r,nat)}P_{i,r}}{\sum_{j\in C} AS_j^{(r,nat)}P_{j,r}}}_{w_i^{(r,nat)}}
\cdot
\underbrace{\frac{P_{i,t}}{P_{i,r}}}_{\text{Price return}}
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

> Interpretation: The Index equals the**base index level**multiplied by the**weighted price change**；Here, $w_i^{(r,nat)}$ denotes the**natural weight**at the rebalancing base time.

## 6. Weighting and Constraints

The Index uses **free-float market capitalization weighting(Free-Float MCAP)** subject to constraints.

### 6.1 Notation and Setup

* $C$：Constituent set at the current rebalancing； $r$：Rebalancing effective time； $t\in[r, r_{\text{next}})$
* $Q_{i,r}$：**Snapshot of the free-float supply**of constituent $i$ at time $r$
* $P_{i,r}$、 $P_{i,t}$：Prices of constituent $i$ 
* $AS_i^{(r,nat)}$：Snapshot of the natural free-float shares of asset $i$ at the current rebalancing base
* $AS_i^{(r,cap)}$：Snapshot of the constrained free-float shares of asset $i$ at the current rebalancing base
* $I_r$：Index level at time $r$； $D$：Corresponding divisor

### 6.2 Natural Weights  $w_i^{\mathrm{nat}}$

Based on the snapshot of unconstrained natural free-float shares（ $AS_i^{(r,nat)}\equiv Q_{i,r}$ ）the natural weight is：

$$
w_i^{(r,nat)}=\frac{AS_i^{(r,nat)}P_{i,r}}{\sum_{j\in C}AS_j^{(r,nat)}P_{j,r}},
\qquad \sum_{i\in C} w_i^{(r,nat)}=1
$$

### 6.3 Applying the 50% Cap and Iterative Redistribution to Obtain Constrained Weights $w_i^{\mathrm{cap}}$

A 50% maximum weight constraint is applied to the natural weights. Any excess weight above 50% is redistributed to unconstrained constituents in proportion to their natural weights, with additional iterations as needed until all weights are $\le 50$%.

Define the capped set $O={i\mid w_i^{\mathrm{nat}}\ge 50}$% and the uncapped set $U=C\setminus O$.

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
  
If any newly computed $w_i^{\mathrm{cap}}>50\%$，$ O,U,E $ are updated and the procedure is repeated until convergence. Small numerical discrepancies may be corrected by a final normalization such that $\sum_i w_i^{\mathrm{cap}}=1$.

### 6.4 Normalization to Ensure $\sum_i w_i^{\mathrm{cap}}=1$

After the computation in Section 6.3, a set of interim constrained weights $\widehat{w}_i$ is obtained such that:
- For the capped set $O$： $\widehat{w}_i\le 50\%$ and typically $\widehat{w}_i=50$%；
- For the uncapped set $U$： $\widehat{w}_i<50$%.

Due to numerical rounding, $\sum_i \widehat{w}_i$ may deviate slightly from 1.To ensure the total sums to 1 without breaking the cap, only the weights in set $U$ are rescaled, while weights in set $O$ remain fixed.

Let:

$$
\sum_i w_i^{\text{cap}} = S_O + \alpha S_U = 1
$$

where:

$$  
S_O = \sum_{i\in O} \widehat{w}_i,\qquad  
S_U = \sum_{i\in U} \widehat{w}_i
$$

gives:

$$
\alpha = \frac{1 - S_O}{S_U}
$$

Thus:

$$
w_i^{\mathrm{cap}} =
\begin{cases}
50％, & i \in O,\\
\alpha\widehat{w}_i, & i \in U.
\end{cases}
$$

### 6.5 Adjusting $AS_i^{(nat)}$ to $AS_i^{(cap)}$

To incorporate the 50% cap and ensure that the constrained weights are exactly reproduced at the base time $r$ , the natural shares $AS_i^{(nat)}$ are updated once at time $r$ to obtain constrained shares $AS_i^{(cap)}$.Thereafter, these constrained shares remain constant over the interval $[r, r_{\text{next}})$, and the Index only evolves with price changes.

$$
AS_i^{(r,cap)} =
\begin{cases}
AS_i^{(r,nat)}\times
\underbrace{\dfrac{w_i^{(r,cap)}}{w_i^{(r,nat)}}}_{\gamma_i\;\text{（weight adjustment ）}}, & w_i^{(r,nat)}>0,\\
0, & w_i^{(r,nat)}=0.
\end{cases}
$$

### 6.6 Index Formula with Constrained Weights

$$
I_t = I_r \cdot \sum_{i\in C} w_i^{(r,cap)} \cdot \frac{P_{i,t}}{P_{i,r}}
$$

## 7. Publication Frequency

* Published in real time,**updated every minute**
* Daily closing level is fixed at UTC 00:00

## 8. Treatment of Crypto “Corporate Actions”

* **Hard Forks**：Treated as spin-offs. The divisor is adjusted; the new chain is not automatically included and will be evaluated at the next reconstitution
* **Airdrops**：Treated as special dividends with corresponding divisor adjustments
* **Mergers / Swaps / Renames**：Adjustments are made to preserve economic equivalence, with divisor changes as needed
* **Burns / Issuance / Unlocks**：Reflected in the supply definition (affecting future weights) but**do not trigger divisor changes**
* **Delistings / Suspensions**：If no reliable price is available, substitutions may be made and the divisor adjusted accordingly

## 9. Governance

* **Index Committee**：Maintains the methodology, approves the list of qualified exchanges, and rules on exceptional cases
* **Methodology Changes**：Material changes are announced at least five business days in advance (except in emergencies)
* **Data Policy**：All data sources and adjustments must be documented and retained for review
* **Annual Review**：Parameters and rules are reviewed at least annually

## 10. Publication and Licensing

* **Codes and Identifiers**：Index code: C10; additional identifiers may be obtained as needed
* **Licensing**：Use of the Index for financial products requires a license agreement
* **Dissemination**：The Index is disseminated via administrator APIs/data feeds and the official website

## 11. Disclaimer

This document is for informational purposes only and does not constitute investment advice or an offer to buy or sell any security or cryptoasset. Cryptoassets are highly volatile and may result in a total loss of value. While the administrator seeks to ensure the accuracy of the Index and this methodology, no guarantee is given, and the methodology may be amended in accordance with the governance policy. Past performance is not indicative of future results.
