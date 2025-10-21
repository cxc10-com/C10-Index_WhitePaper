# C10 Index 加权加密货币指数 — 白皮书（v1.6）

**日期**：2025-10-18
**发起方**：CXC10
**基准点位**：1,000（基准日：2025-08-16 UTC 00:00）
**基准货币**：美元（USD）
**数据来源**：CoinGecko

## 1. 摘要

C10 Index 加权加密货币指数（以下简称“指数”）衡量按**自由流通市值**排名前十的加密资产（不含稳定币、包装代币和质押凭证）的整体表现，并通过**修正市值加权**限制单一资产集中度。指数设定**单一成分权重上限为 50%**，适用于基准衡量、投资组合构建及相关产品复制。

## 2. 指数概览

* **覆盖范围**：符合条件且在合格现货交易所自由交易的加密资产
* **成分数量**：10
* **计价货币**：USD
* **加权方式**：自由流通市值加权 + 50% 上限，超额按比例迭代分配
* **成分调整（重构）**：每季度一次
* **权重调整（再平衡）**：每月一次
* **发布频率**：实时，**每分钟刷新**；每日收盘为 UTC 00:00
* **特殊事件**：分叉、空投、销毁、解锁等通过**除数**或**成分**调整处理

## 3. 纳入与排除标准

### 3.1 纳入标准

1. 至少两家合格交易所上市，市场透明、可审计
2. 可获得 USD 计价价格（直接或通过主流 USD 稳定币对）
3. 流动性：过去 90 天中位日成交额 ≥ 1,000 万美元，且有效交易日 ≥ 85 天
4. 可核实的自由流通供应量数据

### 3.2 排除规则

* 稳定币（USDT、USDC、USDE 等）
* 包装/跨链代币（如 WBTC、WETH、stETH、wstETH、eETH、WBETH 等）
* 存在重大合规或安全风险的资产（委员会裁量）
* 多代码重复经济权益仅保留主表示

## 4. 成分选取、重构与再平衡

### 4.1 季度重构

每季度末倒数第二个工作日（选样日）按**自由流通市值**（USD）排序取前十，数据口径见第 5 节。公告提前 ≥ 3 个工作日发布，新成分于下季度首日 UTC 00:00 生效。

### 4.2 月度再平衡

每月首日 UTC 00:00 依据计算口径**锁定份额**，不更换成分。

### 4.3 非例行调整

退市、长时间停牌、重大安全事件等可临时替换成分，并调整除数保持指数连续。

## 5. 价格与供应口径

### 5.1 价格

* 取合格交易所现货对 USD（含主流 USD 稳定币）的**成交量加权中位价**或 **VWAP（1 分钟窗口）**
* 异常值以稳健统计方法剔除

### 5.2 自由流通供应量

* 采用权威数据源（主要为 CoinGecko），剔除锁仓（团队、基金会）、已知丢失币、重复映射份额等
* 所有调整需留痕记录

### 5.3 市值计算

自由流通市值在时间 $t$ 为：

$$
\text{MarketCap}_{i,t} = Q_{i,t}\cdot P_{i,t}
$$

* $Q_{i,t}$：资产 $i$ 在 $t$ 时刻的**自由流通供应量**
* $P_{i,t}$：资产 $i$ 在 $t$ 时刻的 USD 价格

### 5.4 指数定义

指数在再平衡区间 $[r, r^+)$ 内，以**锁定份额**与**除数**计算：

$$
I_t = \frac{\sum_{i\in C} AS_i^{(r,nat)}\cdot P_{i,t}}{D}
$$

* $C$：本次再平衡的成分集合
* $D$：再平衡时刻对应的除数
* $AS_i^{(r,nat)}$：资产 $i$ 在再平衡基点 $r$ 取各成分的**自然自由流通供应量快照**并锁定，记为：

$$
AS_i^{(r,nat)} \equiv Q_{i,r},
\qquad t\in[r, r_{\text{next}}) \text{ 期间保持不变。}
$$

### 5.5 除数 $D$ 的初始定标

基准日 $t_0$ 指数设定为 $I_{t_0}=1000$：

$$
D_{\text{initial}}
=\frac{\sum_{i\in C_{t_0}} AS_i^{(t_0,nat)}\cdot P_{i,t_0}}{1000}
=\frac{\sum_{i\in C_{t_0}} AS_i^{(t_0,nat)}\cdot P_{i,t_0}}{I_{t_0}}
$$

### 5.6 再平衡时除数 $D$ 的调整

为保证指数在再平衡**接点不跳变**，令

$$
I_r \coloneqq I_{r^-}, \qquad P_{i,r}\coloneqq P_{i,r^-}
$$

- $I_r$：本次再平衡时刻后一瞬间的指数点
- $I_{r-}$：本次再平衡时刻前一瞬间的指数点
- $P_{i,r}$：资产 $i$ 在本次再平衡时刻后一瞬间的USD价格。
- $P_{i,r-}$：资产 $i$ 在本次再平衡时刻前一瞬间的USD价格。

于是

$$
I_{r^-}=\frac{\sum_{i\in C_{\text{old}}} AS_i^{(r^-,nat)}P_{i,r}}{D_{\text{old}}},\qquad
I_{r}=\frac{\sum_{i\in C_{\text{new}}} AS_i^{(r,nat)}P_{i,r}}{D_{\text{new}}}
$$

- $AS_i^{(r,nat)}$：资产 $i$ 在本次再平衡基点的自然自由流通供应量快照
- $AS_i^{(r-,nat)}$：资产 $i$ 在前次再平衡基点的自然自由流通供应量快照

令 $I_r=I_{r^-}$ 可得“比值法”：

$$
D_{\text{new}}
= D_{\text{old}}\cdot
\frac{\sum_{i\in C_{\text{new}}} AS_i^{(r,nat)}P_{i,r}}
{\sum_{i\in C_{\text{old}}} AS_i^{(r^-,nat)}P_{i,r}}
$$

或由定义直接得到：

$$
D_{\text{new}}
=\frac{\sum_{i\in C_{\text{new}}} AS_i^{(r,nat)}P_{i,r}}{I_r}
$$

- $D_{\text{old}}$：前次再平衡的除数
- $D_{\text{new}}$：本次再平衡的除数
- $C_{\text{old}}$：前次再平衡的成分集合
- $C_{\text{new}}$：本次再平衡的成分集合

### 5.7 指数计算方法论

对任一再平衡时刻 $r$ 都有

$$
D=\frac{\sum_{i\in C} AS_i^{(r,nat)}P_{i,r}}{I_r},
$$

从而 5.5 的初始定标（取 $r=t_0$）与 5.6 的再平衡接平在同一表达式下统一。

代入指数计算公式

$$
I_t=\frac{\sum_{i\in C} AS_i^{(r,nat)}\cdot P_{i,t}}{D}
$$

可得

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

于是：

$$
I_t
= I_r \cdot
\sum_{i\in C}\frac{\left(AS_i^{(r,nat)} P_{i,r}\right)\cdot \frac{P_{i,t}}{P_{i,r}}}{\sum_{j\in C} AS_j^{(r,nat)} P_{j,r}}\
= I_r \cdot \sum_{i\in C}
\underbrace{\frac{AS_i^{(r,nat)}P_{i,r}}{\sum_{j\in C} AS_j^{(r,nat)}P_{j,r}}}_{w_i^{(r,nat)}}
\cdot
\underbrace{\frac{P_{i,t}}{P_{i,r}}}_{\text{价格收益}}
$$

令

$$
w_i^{(r,nat)}=\frac{AS_i^{(r,nat)}P_{i,r}}{\sum_{j\in C}AS_j^{(r,nat)}P_{j,r}},
\qquad \sum_{i\in C} w_i^{(r,nat)}=1
$$

则

$$
I_t = I_r \cdot \sum_{i\in C} w_i^{(r,nat)} \cdot \frac{P_{i,t}}{P_{i,r}}
$$

> 解释：指数等于**基期点位**乘以**加权价格变化**；此处 $w_i^{(r,nat)}$ 为再平衡基点的**自然权重**。

## 6. 权重与约束规则

采用 **自由流通市值加权(Free-Float MCAP)** 并施加约束。

### 6.1 符号与前置

* $C$：本次再平衡的成分集合； $r$：再平衡生效时刻； $t\in[r, r_{\text{next}})$。
* $Q_{i,r}$：成分 $i$ 在 $r$ 的**自由流通供应量快照**。
* $P_{i,r}$、 $P_{i,t}$：成分 $i$ 的价格。
* $AS_i^{(r,nat)}$：资产 $i$ 在本次再平衡基点的自然自由流通供应量快照
* $AS_i^{(r,cap)}$：资产 $i$ 在本次再平衡基点的约束自由流通供应量快照
* $I_r$：指数在 $r$ 的点位； $D$：对应除数。

### 6.2 自然权重  $w_i^{\mathrm{nat}}$

根据未施加约束的自然自由流通供应量快照（ $AS_i^{(r,nat)}\equiv Q_{i,r}$ ）得到当期自然权重：

$$
w_i^{(r,nat)}=\frac{AS_i^{(r,nat)}P_{i,r}}{\sum_{j\in C}AS_j^{(r,nat)}P_{j,r}},
\qquad \sum_{i\in C} w_i^{(r,nat)}=1
$$

### 6.3 施加50% 上限与迭代再分配得到约束权重 $w_i^{\mathrm{cap}}$

对自然权重施加单一资产上限 50%，并将超额按未触顶成分的自然权重比例分配，必要时迭代直至全部 $\le 50$%。

定义触顶集合 $O={i\mid w_i^{\mathrm{nat}}\ge 50}$%，未触顶集合 $U=C\setminus O$。

定义超额总量：  

$$
E=\sum_{i\in O}\bigl(w_i^{\mathrm{nat}}-50％\bigr)
$$

则约束权重：

$$
w_i^{\mathrm{cap}} =
\begin{cases}
50％,& i \in O,\\
w_i^{\mathrm{nat}} +
\dfrac{w_i^{\mathrm{nat}}}{\displaystyle\sum_{k\in U} w_k^{\mathrm{nat}}}E, & i \in U.
\end{cases}
$$
  
若有新的 $w_i^{\mathrm{cap}}>50\%$，更新 $O,U,E$ 并重复上式直至收敛。数值上如有微小累积误差，可作一次归一化使 $\sum_i w_i^{\mathrm{cap}}=1$。

### 6.4 归一化使 $\sum_i w_i^{\mathrm{cap}}=1$

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

* **代码与标识符**：代码 C10；其他识别码按需申请
* **授权**：用于金融产品需签署许可
* **发布**：管理员 API/数据源与官网同步

## 11. 免责声明

本文档仅供参考，不构成投资建议或买卖要约。加密资产波动性高，可能损失全部价值。管理员力求准确，但不保证结果，方法论可按治理政策调整。历史表现不代表未来结果。

## 12. 指数计算范例

### 12.1 基准设定与指数公式

以 2025.9.1 为基准日（UTC 0 点），基准指数 $I_{9/1(O)}=1000$。

$$
I_t = I_r \cdot \sum_{i\in C} w_i^{(r,\mathrm{cap})}\cdot \frac{P_{i,t}}{P_{i,r}}
$$

### 12.2 2025年9月再平衡（9/1）

#### 12.2.1 自然权重计算

自然权重（由当刻自由流通市值）：

$$
\begin{aligned}
w_{i}^{(9/1,\mathrm{nat})}
&=\frac{\mathrm{MarketCap}_{i,9/1(O)}}{\displaystyle\sum_{j\in C_{9/1}}\mathrm{MarketCap}_{j,9/1(O)}}
\,\quad
\sum\limits_{j\in C_{9/1}}\mathrm{MarketCap}_{j,9/1(O)}&=3200565092014.6787
\end{aligned}
$$

* **BTC**:
    * $\displaystyle \frac{2155694976689.5984}{3200565092014.6787} = 0.6735357396942176$
* **ETH**:
    * $\displaystyle \frac{529740073153.04834}{3200565092014.6787} = 0.16551454443927266$
* **XRP**:
    * $\displaystyle \frac{165199510162.63672}{3200565092014.6787} = 0.05161573203894679$
* **BNB**:
    * $\displaystyle \frac{119395441256.787}{3200565092014.6787} = 0.03730448774645306$
* **SOL**:
    * $\displaystyle \frac{108641493698.23296}{3200565092014.6787} = 0.033944472483715606$
* **TRX**:
    * $\displaystyle \frac{32256431725.30664}{3200565092014.6787} = 0.010078355164775604$
* **DOGE**:
    * $\displaystyle \frac{32224320280.031105}{3200565092014.6787} = 0.01006832210987675$
* **ADA**:
    * $\displaystyle \frac{29618687472.74614}{3200565092014.6787} = 0.009254205623451917$
* **LINK**:
    * $\displaystyle \frac{15774709649.672829}{3200565092014.6787} = 0.004928726395545053$
* **HYPE**:
    * $\displaystyle \frac{12019447926.61887}{3200565092014.6787} = 0.0037554143037450043$

#### 12.2.2 施加50%上限与迭代再分配

约束权重：

$$
w_i^{\mathrm{cap}} =
\begin{cases}
0.5, & i=\text{BTC}\\
w_i^{(9/1,\mathrm{nat})} +
\dfrac{w_i^{(9/1,\mathrm{nat})}}{\displaystyle\sum_{k\in U} w_k^{\mathrm{nat}}}E, & i \in U
\end{cases}
$$

定义触顶集合 $O=\{\text{BTC}\}$，未触顶集合 $U=C\setminus O$。

$$
E=0.6735357396942176-0.5=0.17353573969421765
$$

$$
\sum_{k\in U} w_k^{\mathrm{nat}}=1-0.6735357396942176=0.32646426030578235
$$

逐项计算：

* **BTC**:
    * $w_{\text{BTC}}^{\mathrm{cap}} = 0.5$
* **ETH**:
    * $\displaystyle \alpha=\dfrac{0.16551454443927266}{0.32646426030578235}=0.5069913144068011$
    * $\displaystyle \Delta=0.17353573969421765\times 0.5069913144068011=0.0879811127641279$
    * $\displaystyle w_{\text{ETH}}^{\mathrm{cap}}=0.16551454443927266+0.0879811127641279=0.25349565720340056$
* **XRP**:
    * $\displaystyle \alpha=\dfrac{0.05161573203894679}{0.32646426030578235}=0.1581053068124547$
    * $\displaystyle \Delta=0.17353573969421765\times 0.1581053068124547=0.027436921367280553$
    * $\displaystyle w_{\text{XRP}}^{\mathrm{cap}}=0.05161573203894679+0.027436921367280553=0.07905265340622734$
* **BNB**:
    * $\displaystyle \alpha=\dfrac{0.03730448774645306}{0.32646426030578235}=0.11426821334596277$
    * $\displaystyle \Delta=0.17353573969421765\times 0.11426821334596277=0.019829618926528323$
    * $\displaystyle w_{\text{BNB}}^{\mathrm{cap}}=0.03730448774645306+0.019829618926528323=0.05713410667298138$
* **SOL**:
    * $\displaystyle \alpha=\dfrac{0.033944472483715606}{0.32646426030578235}=0.10397607521240321$
    * $\displaystyle \Delta=0.17353573969421765\times 0.10397607521240321=0.018043565122485998$
    * $\displaystyle w_{\text{SOL}}^{\mathrm{cap}}=0.033944472483715606+0.018043565122485998=0.051988037606201604$
* **TRX**:
    * $\displaystyle \alpha=\dfrac{0.010078355164775604}{0.32646426030578235}=0.030871235814100215$
    * $\displaystyle \Delta=0.17353573969421765\times 0.030871235814100215=0.005357262742274504$
    * $\displaystyle w_{\text{TRX}}^{\mathrm{cap}}=0.010078355164775604+0.005357262742274504=0.015435617907050107$
* **DOGE**:
    * $\displaystyle \alpha=\dfrac{0.01006832210987675}{0.32646426030578235}=0.030840503338546977$
    * $\displaystyle \Delta=0.17353573969421765\times 0.030840503338546977=0.005351929559396738$
    * $\displaystyle w_{\text{DOGE}}^{\mathrm{cap}}=0.01006832210987675+0.005351929559396738=0.015420251669273487$
* **ADA**:
    * $\displaystyle \alpha=\dfrac{0.009254205623451917}{0.32646426030578235}=0.0283467648641967$
    * $\displaystyle \Delta=0.17353573969421765\times 0.0283467648641967=0.004919176808646434$
    * $\displaystyle w_{\text{ADA}}^{\mathrm{cap}}=0.009254205623451917+0.004919176808646434=0.01417338243209835$
* **LINK**:
    * $\displaystyle \alpha=\dfrac{0.004928726395545053}{0.32646426030578235}=0.015097292398648989$
    * $\displaystyle \Delta=0.17353573969421765\times 0.015097292398648989=0.002619919803779442$
    * $\displaystyle w_{\text{LINK}}^{\mathrm{cap}}=0.004928726395545053+0.002619919803779442=0.007548646199324495$
* **HYPE**:
    * $\displaystyle \alpha=\dfrac{0.0037554143037450043}{0.32646426030578235}=0.01150329380688563$
    * $\displaystyle \Delta=0.17353573969421765\times 0.01150329380688563=0.0019962325996978106$
    * $\displaystyle w_{\text{HYPE}}^{\mathrm{cap}}=0.0037554143037450043+0.0019962325996978106=0.0057516469034428145$

校验求和：

$$
\sum_i w_{i}^{\mathrm{cap}}
= 0.5+0.25349565720340056+0.07905265340622734+0.05713410667298138+0.051988037606201604\\
+0.015435617907050107+0.015420251669273487+0.01417338243209835\\
+0.007548646199324495+0.0057516469034428145
= 1.0000000000000002
$$

#### 12.2.3 权重归一化使 $\sum_i w_i^{\mathrm{cap}}=1$

经过 6.3 节计算，得到一组暂态约束权重 $\widehat{w}_i^{9/1}$（即上节输出的 $w_i^{\mathrm{cap}}$），其中

$$
\sum_i \widehat{w}_{i}^{9/1}=\mathbf{1.0000000000000002}
$$

$$
S_O=\sum_{i\in O}\widehat{w}_{i}^{9/1}=\mathbf{0.5}
$$

$$
S_U=\sum_{i\in U}\widehat{w}_{i}^{9/1}=\mathbf{0.5000000000000002}
$$

归一化系数（只对"非 BTC"部分做等比缩放）：

$$
\alpha
=\frac{1-S_O}{S_U}
=\frac{0.5}{0.5000000000000002}
=\mathbf{0.9999999999999996}
$$

最终权重：

$$
w_i^{(9/1,\mathrm{cap})} =
\begin{cases}
0.5, & i\in O\\
\alpha\cdot\widehat{w}_{i}^{9/1}, & i\in U
\end{cases}
$$

逐项归一化结果（完整小数）：

* **BTC**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{BTC}} = \mathbf{0.5}$
* **ETH**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{ETH}}\times \alpha = 0.25349565720340056 \times 0.9999999999999996 = \mathbf{0.25349565720340045}$
* **XRP**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{XRP}}\times \alpha = 0.07905265340622734 \times 0.9999999999999996 = \mathbf{0.0790526534062273}$
* **BNB**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{BNB}}\times \alpha = 0.05713410667298138 \times 0.9999999999999996 = \mathbf{0.057134106672981355}$
* **SOL**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{SOL}}\times \alpha = 0.051988037606201604 \times 0.9999999999999996 = \mathbf{0.05198803760620158}$
* **TRX**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{TRX}}\times \alpha = 0.015435617907050107 \times 0.9999999999999996 = \mathbf{0.0154356179070501}$
* **DOGE**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{DOGE}}\times \alpha = 0.015420251669273487 \times 0.9999999999999996 = \mathbf{0.01542025166927348}$
* **ADA**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{ADA}}\times \alpha = 0.01417338243209835 \times 0.9999999999999996 = \mathbf{0.014173382432098343}$
* **LINK**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{LINK}}\times \alpha = 0.007548646199324495 \times 0.9999999999999996 = \mathbf{0.007548646199324492}$
* **HYPE**:
  * $\displaystyle \widehat{w}^{9/1}_{\text{HYPE}}\times \alpha = 0.0057516469034428145 \times 0.9999999999999996 = \mathbf{0.005751646903442812}$


归一化校验（构造上精确满足）：

$$
\sum_{i}w_i^{(9/1,\mathrm{cap})}
=\underbrace{S_O\times 1}_{=0.5}
+\underbrace{S_U\times \alpha}_{=0.5000000000000002\times 0.9999999999999996=0.5}
=\mathbf{1}
$$

> 说明：这样处理严格满足"BTC=50% 不变"的设限，同时把其余成分按同一系数 $\alpha$ 等比缩放，使总权重精确为 1。

#### 12.2.4 9/1收盘指数计算

公式

$$
I_{9/1}
= I_{9/1(O)}\cdot \sum_{i\in C_{9/1}} w_i^{(9/1,\mathrm{cap})}\cdot
\frac{P_{i,\mathbf{9/1}}}{P_{i,\mathbf{9/1(O)}}},\qquad
I_{9/1(O)}=1000
$$

> 注：**“9/1 收盘价”= 9/2 00:00 UTC。O为开盘价。**

* **BTC**:
  * $\displaystyle w_{\text{BTC}}^{(9/1,\mathrm{cap})}=0.5$
  * $\displaystyle \frac{P_{\text{BTC},9/1}}{P_{\text{BTC},9/1(O)}}
    =\frac{\mathbf{109162.68557992298}}{\mathbf{108253.36092385623}}
    =\mathbf{1.0083999669692136}$
  * $\displaystyle w_{\text{BTC}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.5041999834846068}$
* **ETH**:
  * $\displaystyle w_{\text{ETH}}^{(9/1,\mathrm{cap})}=0.25349565720340045$
  * $\displaystyle \frac{P_{\text{ETH},9/1}}{P_{\text{ETH},9/1(O)}}
    =\frac{\mathbf{4303.202222745541}}{\mathbf{4388.931464519364}}
    =\mathbf{0.9804669445246825}$
  * $\displaystyle w_{\text{ETH}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.24854411246849437}$
* **XRP**:
  * $\displaystyle w_{\text{XRP}}^{(9/1,\mathrm{cap})}=0.0790526534062273$
  * $\displaystyle \frac{P_{\text{XRP},9/1}}{P_{\text{XRP},9/1(O)}}
    =\frac{\mathbf{2.752094842681695}}{\mathbf{2.776712609891609}}
    =\mathbf{0.9901280646537916}$
  * $\displaystyle w_{\text{XRP}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.07827852787876354}$
* **BNB**:
  * $\displaystyle w_{\text{BNB}}^{(9/1,\mathrm{cap})}=0.057134106672981355$
  * $\displaystyle \frac{P_{\text{BNB},9/1}}{P_{\text{BNB},9/1(O)}}
    =\frac{\mathbf{1005.574831704243}}{\mathbf{1002.251682003274}}
    =\mathbf{1.0033150266045858}$
  * $\displaystyle w_{\text{BNB}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.05732380867156660}$
* **SOL**:
  * $\displaystyle w_{\text{SOL}}^{(9/1,\mathrm{cap})}=0.05198803760620158$
  * $\displaystyle \frac{P_{\text{SOL},9/1}}{P_{\text{SOL},9/1(O)}}
    =\frac{\mathbf{197.66270682973027}}{\mathbf{205.12838610102386}}
    =\mathbf{0.9635576989422688}$
  * $\displaystyle w_{\text{SOL}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.05007036893063023}$
* **TRX**:
  * $\displaystyle w_{\text{TRX}}^{(9/1,\mathrm{cap})}=0.0154356179070501$
  * $\displaystyle \frac{P_{\text{TRX},9/1}}{P_{\text{TRX},9/1(O)}}
    =\frac{\mathbf{0.323681332993614}}{\mathbf{0.332362570767100}}
    =\mathbf{0.9738575385126106}$
  * $\displaystyle w_{\text{TRX}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.01503470306394761}$
* **DOGE**:
  * $\displaystyle w_{\text{DOGE}}^{(9/1,\mathrm{cap})}=0.01542025166927348$
  * $\displaystyle \frac{P_{\text{DOGE},9/1}}{P_{\text{DOGE},9/1(O)}}
    =\frac{\mathbf{0.241441506427685}}{\mathbf{0.243231957517967}}
    =\mathbf{0.9926462740767630}$
  * $\displaystyle w_{\text{DOGE}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.01530754444936604}$
* **ADA**:
  * $\displaystyle w_{\text{ADA}}^{(9/1,\mathrm{cap})}=0.014173382432098343$
  * $\displaystyle \frac{P_{\text{ADA},9/1}}{P_{\text{ADA},9/1(O)}}
    =\frac{\mathbf{0.803044535123922}}{\mathbf{0.814881556780723}}
    =\mathbf{0.9853985136344930}$
  * $\displaystyle w_{\text{ADA}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.013959113371730603}$
* **LINK**:
  * $\displaystyle w_{\text{LINK}}^{(9/1,\mathrm{cap})}=0.007548646199324492$
  * $\displaystyle \frac{P_{\text{LINK},9/1}}{P_{\text{LINK},9/1(O)}}
    =\frac{\mathbf{22.440918059371164}}{\mathbf{23.229075217478396}}
    =\mathbf{0.9660702309184399}$
  * $\displaystyle w_{\text{LINK}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.007292522376903016}$
* **HYPE**:
  * $\displaystyle w_{\text{HYPE}}^{(9/1,\mathrm{cap})}=0.005751646903442812$
  * $\displaystyle \frac{P_{\text{HYPE},9/1}}{P_{\text{HYPE},9/1(O)}}
    =\frac{\mathbf{43.030960057683984}}{\mathbf{44.30452855074537}}
    =\mathbf{0.9712542140787556}$
  * $\displaystyle w_{\text{HYPE}}^{(9/1,\mathrm{cap})}\times\frac{P_{9/1}}{P_{9/1(O)}}=\mathbf{0.005586311292861857}$

求和与指数：

$$
\sum_i w_i^{(9/1,\mathrm{cap})}\cdot\frac{P_{i,9/1}}{P_{i,9/1(O)}}
=\mathbf{0.9956529534972314798}
$$

$$
I_{9/1}
=1000\times \mathbf{0.9956529534972314798}
=\boxed{\mathbf{995.6529534972315}}
$$

> 解释：这是 **9/1 一天的收盘指数**（区间 9/01 00:00 → 9/02 00:00，UTC），**沿用 9/1 权重**按“收盘/开盘”的线性相对法滚动得到的数值。

#### 12.2.5 9/30收盘指数计算

$$
I_{9/30} = I_{9/1(O)}\cdot \sum_{i\in C_{9/1}} w_i^{(9/1,\mathrm{cap})} \cdot \frac{P_{i,9/30}}{P_{i,9/1(O)}},
\qquad I_{9/1(O)}=1000
$$

逐项计算：

* **BTC**:
    * $\displaystyle w_{\text{BTC}}^{(9/1,\mathrm{cap})}=0.5$
    * $\displaystyle \frac{P_{\text{BTC},9/30}}{P_{\text{BTC},9/1(O)}}=1.0533088937137336$
    * $\displaystyle w_{\text{BTC}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{BTC},9/30}}{P_{\text{BTC},9/1(O)}}=0.5266544468568668$
* **ETH**:
    * $\displaystyle w_{\text{ETH}}^{(9/1,\mathrm{cap})}=0.25349565720340045$
    * $\displaystyle \frac{P_{\text{ETH},9/30}}{P_{\text{ETH},9/1(O)}}=0.9442458823688693$
    * $\displaystyle w_{\text{ETH}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{ETH},9/30}}{P_{\text{ETH},9/1(O)}}=0.23936223051270128$
* **XRP**:
    * $\displaystyle w_{\text{XRP}}^{(9/1,\mathrm{cap})}=0.0790526534062273$
    * $\displaystyle \frac{P_{\text{XRP},9/30}}{P_{\text{XRP},9/1(O)}}=1.0246069432522836$
    * $\displaystyle w_{\text{XRP}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{XRP},9/30}}{P_{\text{XRP},9/1(O)}}=0.08100184744649337$
* **BNB**:
    * $\displaystyle w_{\text{BNB},9/30}^{(9/1,\mathrm{cap})}=0.057134106672981355$
    * $\displaystyle \frac{P_{\text{BNB},9/30}}{P_{\text{BNB},9/1(O)}}=1.1958694450416485$
    * $\displaystyle w_{\text{BNB}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{BNB},9/30}}{P_{\text{BNB},9/1(O)}}=0.06836331852592957$
* **SOL**:
    * $\displaystyle w_{\text{SOL}}^{(9/1,\mathrm{cap})}=0.05198803760620158$
    * $\displaystyle \frac{P_{\text{SOL},9/30}}{P_{\text{SOL},9/1(O)}}=1.0769681579199496$
    * $\displaystyle w_{\text{SOL}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{SOL},9/30}}{P_{\text{SOL},9/1(O)}}=0.05599265924895757$
* **TRX**:
    * $\displaystyle w_{\text{TRX}}^{(9/1,\mathrm{cap})}=0.0154356179070501$
    * $\displaystyle \frac{P_{\text{TRX},9/30}}{P_{\text{TRX},9/1(O)}}=1.045951045002095$
    * $\displaystyle w_{\text{TRX}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{TRX},9/30}}{P_{\text{TRX},9/1(O)}}=0.01614138329095509$
* **DOGE**:
    * $\displaystyle w_{\text{DOGE}}^{(9/1,\mathrm{cap})}=0.01542025166927348$
    * $\displaystyle \frac{P_{\text{DOGE},9/30}}{P_{\text{DOGE},9/1(O)}}=1.0645391372128677$
    * $\displaystyle w_{\text{DOGE}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{DOGE},9/30}}{P_{\text{DOGE},9/1(O)}}=0.01641734102926561$
* **ADA**:
    * $\displaystyle w_{\text{ADA}}^{(9/1,\mathrm{cap})}=0.014173382432098343$
    * $\displaystyle \frac{P_{\text{ADA},9/30}}{P_{\text{ADA},9/1(O)}}=0.9940505247028603$
    * $\displaystyle w_{\text{ADA}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{ADA},9/30}}{P_{\text{ADA},9/1(O)}}=0.014089058243441659$
* **LINK**:
    * $\displaystyle w_{\text{LINK}}^{(9/1,\mathrm{cap})}=0.007548646199324492$
    * $\displaystyle \frac{P_{\text{LINK},9/30}}{P_{\text{LINK},9/1(O)}}=0.9181080365364191$
    * $\displaystyle w_{\text{LINK}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{LINK},9/30}}{P_{\text{LINK},9/1(O)}}=0.006930472740569912$
* **HYPE**:
    * $\displaystyle w_{\text{HYPE}}^{(9/1,\mathrm{cap})}=0.005751646903442812$
    * $\displaystyle \frac{P_{\text{HYPE},9/30}}{P_{\text{HYPE},9/1(O)}}=1.0211971868882697$
    * $\displaystyle w_{\text{HYPE}}^{(9/1,\mathrm{cap})}\cdot\frac{P_{\text{HYPE},9/30}}{P_{\text{HYPE},9/1(O)}}=0.005873565637770427$


求和与指数：

$$
\sum_i w_i^{(9/1,\mathrm{cap})}\cdot\frac{P_{i,9/30}}{P_{i,9/1(O)}}
= \mathbf{1.027002814493407}
$$

$$
I_{9/30}=1000\times 1.027002814493407
=\boxed{1027.002814493407}
$$

> 说明：数值仅在 $10^{-13}$ 量级微调（由归一化引起），口径上满足"**BTC=50% 严格不变**、其余等比缩放、**权重和=1**"。


### 12.3 2025年10月再平衡（10/1）

#### 12.3.1 10月的权重触顶后，按比例分配完的权重为：

* **BTC**:
  * $\displaystyle w_{\text{BTC}}^{(10/1,\mathrm{cap})}=0.5$
* **ETH**:
  * $\displaystyle w_{\text{ETH}}^{(10/1,\mathrm{cap})}=0.23885866123882363$
* **XRP**:
  * $\displaystyle w_{\text{XRP}}^{(10/1,\mathrm{cap})}=0.08126670985348254$
* **BNB**:
  * $\displaystyle w_{\text{BNB}}^{(10/1,\mathrm{cap})}=0.06703093399646864$
* **SOL**:
  * $\displaystyle w_{\text{SOL}}^{(10/1,\mathrm{cap})}=0.05416309452567625$
* **TRX**:
  * $\displaystyle w_{\text{TRX}}^{(10/1,\mathrm{cap})}=0.015073605946657255$
* **DOGE**:
  * $\displaystyle w_{\text{DOGE}}^{(10/1,\mathrm{cap})}=0.016795357217090846$
* **ADA**:
  * $\displaystyle w_{\text{ADA}}^{(10/1,\mathrm{cap})}=0.014054728615644059$
* **LINK**:
  * $\displaystyle w_{\text{LINK}}^{(10/1,\mathrm{cap})}=0.00690712909513855$
* **HYPE**:
  * $\displaystyle w_{\text{HYPE}}^{(10/1,\mathrm{cap})}=0.005849779511018379$

#### 12.3.2 10/1收盘指数计算（**使用 10/1 新权重**）

$$
I_{10/1}
= I_{10/1(O)}\cdot
\sum_{i\in C} w_i^{(10/1,\mathrm{cap})}\cdot
\frac{P_{i,10/1}}{P_{i,10/1(O)}},
\qquad
I_{9/30}=I_{10/1(O)}=\mathbf{1027.002814493407}
$$

* **BTC**:
  * $\displaystyle w_{\text{BTC}}^{(10/1,\mathrm{cap})}=0.5$
  * $\displaystyle \frac{P_{\text{BTC},10/1}}{P_{\text{BTC},10/1(O)}}
    =\frac{\mathbf{118503.244517524843104}}{\mathbf{114024.227835500540095}}
    =\mathbf{1.039281271770470}$
  * $\displaystyle w_{\text{BTC}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.519640635885235}$
* **ETH**:
  * $\displaystyle w_{\text{ETH}}^{(10/1,\mathrm{cap})}=0.23885866123882363$
  * $\displaystyle \frac{P_{\text{ETH},10/1}}{P_{\text{ETH},10/1(O)}}
    =\frac{\mathbf{4343.951983440489130}}{\mathbf{4144.230463371581209}}
    =\mathbf{1.048192667332121}$
  * $\displaystyle w_{\text{ETH}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.250369897239302}$
* **XRP**:
  * $\displaystyle w_{\text{XRP}}^{(10/1,\mathrm{cap})}=0.08126670985348254$
  * $\displaystyle \frac{P_{\text{XRP},10/1}}{P_{\text{XRP},10/1(O)}}
    =\frac{\mathbf{2.944797512076650}}{\mathbf{2.844933401817261}}
    =\mathbf{1.035102442185676}$
  * $\displaystyle w_{\text{XRP}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.084119369837735}$
* **BNB**:
  * $\displaystyle w_{\text{BNB}}^{(10/1,\mathrm{cap})}=0.06703093399646864$
  * $\displaystyle \frac{P_{\text{BNB},10/1}}{P_{\text{BNB},10/1(O)}}
    =\frac{\mathbf{1025.818785012324724}}{\mathbf{1008.922531171410355}}
    =\mathbf{1.016746829730621}$
  * $\displaystyle w_{\text{BNB}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.068153489634792}$
* **SOL**:
  * $\displaystyle w_{\text{SOL}}^{(10/1,\mathrm{cap})}=0.05416309452567625$
  * $\displaystyle \frac{P_{\text{SOL},10/1}}{P_{\text{SOL},10/1(O)}}
    =\frac{\mathbf{221.218883338975502}}{\mathbf{208.699294488173308}}
    =\mathbf{1.059988649609506}$
  * $\displaystyle w_{\text{SOL}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.057412265424944}$
* **TRX**:
  * $\displaystyle w_{\text{TRX}}^{(10/1,\mathrm{cap})}=0.015073605946657255$
  * $\displaystyle \frac{P_{\text{TRX},10/1}}{P_{\text{TRX},10/1(O)}}
    =\frac{\mathbf{0.341791716791851}}{\mathbf{0.333500157269788}}
    =\mathbf{1.024862235718094}$
  * $\displaystyle w_{\text{TRX}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.015448369490825}$
* **DOGE**:
  * $\displaystyle w_{\text{DOGE}}^{(10/1,\mathrm{cap})}=0.016795357217090846$
  * $\displaystyle \frac{P_{\text{DOGE},10/1}}{P_{\text{DOGE},10/1(O)}}
    =\frac{\mathbf{0.248066930177501}}{\mathbf{0.232770385301574}}
    =\mathbf{1.065715167572151}$
  * $\displaystyle w_{\text{DOGE}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.017899066931046}$
* **ADA**:
  * $\displaystyle w_{\text{ADA}}^{(10/1,\mathrm{cap})}=0.014054728615644059$
  * $\displaystyle \frac{P_{\text{ADA},10/1}}{P_{\text{ADA},10/1(O)}}
    =\frac{\mathbf{0.849177301515390}}{\mathbf{0.806863785307213}}
    =\mathbf{1.052441957339883}$
  * $\displaystyle w_{\text{ADA}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.014791786094129}$
* **LINK**:
  * $\displaystyle w_{\text{LINK}}^{(10/1,\mathrm{cap})}=0.00690712909513855$
  * $\displaystyle \frac{P_{\text{LINK},10/1}}{P_{\text{LINK},10/1(O)}}
    =\frac{\mathbf{22.577894311182035}}{\mathbf{21.326800638475884}}
    =\mathbf{1.058662979689933}$
  * $\displaystyle w_{\text{LINK}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.007312321868962}$
* **HYPE**:
  * $\displaystyle w_{\text{HYPE}}^{(10/1,\mathrm{cap})}=0.005849779511018379$
  * $\displaystyle \frac{P_{\text{HYPE},10/1}}{P_{\text{HYPE},10/1(O)}}
    =\frac{\mathbf{47.111017596069502}}{\mathbf{45.243659922432194}}
    =\mathbf{1.041273355799217}$
  * $\displaystyle w_{\text{HYPE}}^{(10/1,\mathrm{cap})}\times\frac{P_{10/1}}{P_{10/1(O)}}=\mathbf{0.006091219542124}$


求和与指数：

$$
\sum_i w_i^{(10/1,\mathrm{cap})}\cdot\frac{P_{i,10/1}}{P_{i,10/1(O)}}
=\boxed{\mathbf{1.041238421949093}}
$$


$$
I_{10/1}
=\mathbf{1027.002814493407}\times \mathbf{1.041238421949093}
=\boxed{\mathbf{1069.354789900392545}}
$$

