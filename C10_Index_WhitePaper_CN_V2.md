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
