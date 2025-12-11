# C10 Index Free-Float Market-Cap Weighted Cryptocurrency Index — Whitepaper

**Date**：2025-10-18
**Sponsor**：AIxCrypto
**Base Value**：1,000（Base Date：2025-08-16 UTC 00:00）
**Base Currency**：US Dollar（USD）
**Data Source**：CoinGecko

## 1. Overview

The C10 **Free-Float Market-Cap** Weighted Cryptocurrency Index (the “Index”) measures the performance of the top ten cryptoassets by free-float market capitalization, excluding stablecoins, wrapped tokens, and staking receipts. The Index uses a **modified free-float market capitalization weighting** scheme with a **50% single-constituent cap to limit concentration in any one asset**, making it suitable for benchmarking, portfolio construction, and index-linked products.

## 2. Index Summary

* **Universe**：Cryptoassets that meet the eligibility criteria and are freely tradable on qualified spot exchanges
* **Number of Constituents**：10
* **Index Currency**：USD
* **Weighting Method**：Free-float market-cap weighting with a 50% cap; excess weight is redistributed iteratively on a pro-rata basis
* **Constituent Review (Reconstitution)**：Quarterly
* **Weight Review (Rebalancing)**：Monthly
* **Publication Frequency**：Real-time，**updated every minute** ；daily closing level fixed at UTC 00:00
* **Corporate Actions and Special Events**：Forks, airdrops, burns, unlocks and similar events are handled through**divisor**or**constituent**adjustments

## 3. Inclusion and Exclusion Criteria

### 3.1 Inclusion Criteria

* Listed on at least two qualified exchanges that provide transparent and auditable market data
* A reliable USD-denominated price is available, either directly or via a major USD stablecoin trading pair
* Liquidity: the median daily trading volume over the past 90 calendar days is at least USD 10 million, with at least 85 days of non-zero trading
* Free-float supply data can be independently verified
* On an initial and continuing basis, the underlying cryptoasset trades on an Intermarket Surveillance Group (“ISG”) member market from which the listing exchange can obtain trading and surveillance information
* On an initial and continuing basis, the underlying cryptoasset underlies a futures contract that has traded for at least six months on a Designated Contract Market (“DCM”) with which the listing exchange maintains a comprehensive surveillance-sharing arrangement (directly or through joint ISG membership)
* On an initial basis, there exists an exchange-traded fund (“ETF”) listed on a national securities exchange that maintains economic exposure of at least 40% of its net asset value to the asset

### 3.2 Exclusion Rules

* Stablecoins（USDT、USDC、USDE etc.）
* Wrapped or cross-chain representations of other assets（WBTC、WETH、stETH、wstETH、eETH、WBETH etc.）
* Assets subject to material regulatory, legal, or security risks (at the discretion of the Index Committee)
* Multiple tickers representing the same underlying economic exposure, in which case only the primary representation is retained

## 4. Constituent Selection, Reconstitution, and Rebalancing

### 4.1 Quarterly Reconstitution

On the second-to-last business day of each quarter (the “Selection Date”), the Index Universe is ranked by USD **free-float market capitalization** and the top ten assets are selected as constituents, using the data definitions in Section 5. Changes are announced at least three business days in advance and take effect at 00:00 UTC on the first day of the following quarter.

### 4.2 Monthly Rebalancing

On the first calendar day of each month at 00:00 UTC, constituent shares are locked in according to the methodology, and the constituent set remains unchanged until the next rebalancing or reconstitution.

### 4.3 Non-Regular Adjustments

In the event of a delisting, prolonged trading suspension, or major security incident, the Index may implement ad-hoc constituent substitutions and divisor adjustments to preserve index continuity.

## 5. Price and Supply Conventions

### 5.1 Price

* Prices are derived from qualified spot exchanges quoting against USD (including major USD stablecoin pairs), using a**volume-weighted median price**or **VWAP (1-minute window)**
* Outliers are removed using robust statistical methods

### 5.2 Free-Float Supply

* Free-float supply is obtained from reputable data sources (primarily CoinGecko), with adjustments for locked tokens (team and foundation holdings), known lost coins, duplicate representations, etc
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
* $P_{i,r}$,$P_{i,t}$：Prices of constituent $i$ 
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

A $50$% single-constituent cap is imposed on individual natural weights. Any excess above $50$% is redistributed to unconstrained constituents in proportion to their natural weights, iterating as needed until all weights are at or below $\le 50$%.

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
  
If any newly computed $w_i^{\mathrm{cap}}>50\%$, $O$,$U$, and $E$ are updated and the procedure is repeated until convergence. Small numerical discrepancies may be corrected by a final normalization such that $\sum_i w_i^{\mathrm{cap}}=1$.

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

To impose the $50$% single-constituent cap, the natural shares $AS_i^{(r,nat)}$ are updated once at time $r$ to constrained shares $AS_i^{(r,cap)}$.Thereafter, $AS_i^{(r,cap)}$ is held constant over $[r, r_{\text{next}})$, and the Index only evolves with price changes.

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

This document is for informational purposes only and does not constitute investment advice or an offer to buy or sell any cryptoasset. Cryptoassets are highly volatile and may result in a total loss of value. The Index methodology may be changed under the applicable governance policy. Past performance is not indicative of future results.

## 12. Performance & Risk Characteristics

This section summarizes the standard return and risk measures commonly used to describe the historical behavior of the Index. All metrics are computed using the official daily closing Index level at 00:00 UTC, unless otherwise stated.

### 12.1 Return Measures

Let $I_t$ denote the official Index level on calendar day $t$ and $R_t$ the simple daily return.

**Daily Return**

$$
R_t = \frac{I_t}{I_{t-1}} - 1
$$

**Cumulative (Total) Return**

Over a horizon from $t_0$ to $T$:

$$
R_{\text{tot}}(t_0, T) = \frac{I_T}{I_{t_0}} - 1
$$

**Annualized Return**

For a period of $N$ calendar days with simple daily returns $\{R_t\}_{t=1}^N$, the annualized return on a 365-day basis is defined as:

$$
R_{\text{ann}} = \left(\prod_{t=1}^{N} (1+R_t)\right)^{\frac{365}{N}} - 1
$$

In practice, the Index Administrator may compute and publish trailing 1-month, 3-month, 6-month, 1-year and since-inception performance, based on the above conventions.

### 12.2 Volatility

Volatility is measured as the standard deviation of daily returns and is annualized on a 365-day basis.

Let $\bar{R}$ denote the sample mean of daily returns over $N$ calendar days:

$$
\bar{R} = \frac{1}{N}\sum_{t=1}^{N} R_t
$$

**Daily Volatility**

$$
\sigma_{\text{daily}} = \sqrt{\frac{1}{N-1}\sum_{t=1}^{N} (R_t - \bar{R})^2}
$$

**Annualized Volatility**

$$
\sigma_{\text{ann}} = \sigma_{\text{daily}}\cdot\sqrt{365}
$$

Volatility is typically reported as trailing annualized volatility over standard lookback windows (e.g., 30, 90, 180 and 365 calendar days).

### 12.3 Drawdowns and Maximum Drawdown

Drawdown measures the decline of the Index from its previous historical peak.

Let

$$
H_t = \max_{0\leq \tau \leq t} I_\tau
$$

denote the running historical high of the Index up to day $t$.

**Drawdown at Time $t$**

$$
DD_t = \frac{I_t - H_t}{H_t}
$$

Drawdown is typically expressed as a negative percentage.

**Maximum Drawdown Over a Period**

Over a horizon from $t_0$ to $T$:

$$
\text{MaxDD}(t_0, T) = \min_{t_0 \leq t \leq T} DD_t
$$

Maximum drawdown summarizes the largest peak-to-trough decline observed over the specified period and is a standard risk measure for highly volatile asset classes such as cryptoassets.

### 12.4 Correlation and Beta

Correlation and beta are used to characterize the relationship between the Index and other benchmarks (for example, single-asset crypto benchmarks or broader crypto market indices).

Let $R_t^{\text{Index}}$ denote the daily returns of the Index and $R_t^{\text{Ref}}$ the daily returns of a reference benchmark, both measured over the same period of $N$ days.

**Correlation**

$$
\rho = \frac{\operatorname{Cov}\left(R_t^{\text{Index}}, R_t^{\text{Ref}}\right)}{\sigma\left(R_t^{\text{Index}}\right)\,\sigma\left(R_t^{\text{Ref}}\right)}
$$

**Beta**

$$
\beta = \frac{\operatorname{Cov}\left(R_t^{\text{Index}}, R_t^{\text{Ref}}\right)}{\operatorname{Var}\left(R_t^{\text{Ref}}\right)}
$$

These statistics are typically computed using simple daily returns and may be reported on a trailing basis for standard horizons.

### 12.5 Concentration and Turnover

The Index uses a free-float market capitalization weighting scheme subject to a 50% single-constituent cap, as described in Section 6. Concentration and turnover metrics provide additional insight into the Index’s structural risk profile.

**Weight Concentration**

A simple measure of concentration is the Herfindahl–Hirschman Index (“HHI”) based on the constrained weights $w_i^{(r,\mathrm{cap})}$ at rebalancing time $r$:

$$
\text{HHI}_r = \sum_{i\in C} \left(w_i^{(r,\mathrm{cap})}\right)^2
$$

Higher values of $\text{HHI}$ indicate greater concentration in fewer constituents. The 50% cap limits single-asset concentration and helps maintain a minimum level of diversification.

**Turnover**

Portfolio turnover between two consecutive rebalancing dates $r$ and $r_{\text{next}}$ is measured as:

$$
\text{Turnover}(r, r_{\text{next}})
= \frac{1}{2}\sum_{i\in C} \left| w_i^{(r_{\text{next}},\mathrm{cap})} - w_i^{(r,\mathrm{cap})} \right|
$$

Turnover reflects the extent of weight changes required by the methodology and is relevant for index-tracking products, as higher turnover may imply higher trading costs.

### 12.6 Backtested Performance and Limitations

Prior to the official start (live) date of the Index, historical Index levels may be calculated on a backtested basis using the same methodology, rules, and data sources as described in this document. Backtested results are purely hypothetical and are subject to a number of important limitations, including but not limited to:

* Backtested performance is generated with the benefit of hindsight and does not reflect actual trading, liquidity constraints, transaction costs, or operational frictions.  
* Backtested performance may differ materially from the performance of any live index or investable product that seeks to track the Index.  
* Methodology changes, data revisions, and corporate actions may affect historical levels if implemented on a restated basis.  

Any performance and risk statistics derived from backtested data should therefore be viewed as illustrative only and not as a guarantee or indication of future results.
