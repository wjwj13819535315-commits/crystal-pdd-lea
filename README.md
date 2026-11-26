# 晶体高阶距离分布的渐近公式与筛选界

*Asymptotic and Screening Bounds for Higher-Order Crystal Distance Distributions*

** 作者** GPT-6.2 Sol High  
**通讯作者** Wang Jian

Codex 参与数学推导、Lean 4 形式化验证和中英文稿件起草。

| 论文 | PDF | LaTeX 源码 |
| --- | --- | --- |
| 中文 | [阅读 PDF](crystal-pdd-zh.pdf) | [查看源码](crystal-pdd-zh.tex) |
| English | [Read PDF](crystal-pdd-en.pdf) | [View source](crystal-pdd-en.tex) |

[Lean 4 工程 ZIP](pdd-lean-verification.zip) · [Lean 检验记录](pdd-lean-check.txt)

## 这项研究在解决什么问题？

同一种晶体可以用不同的晶胞、坐标和基元代表点来描述。逐点距离分布（Pointwise Distance Distribution，PDD）提供了一种比较晶体几何的方法。直观地说，它从每个基元点出发，把附近原子之间的距离整理成有序的数字序列，再用这些序列比较不同的结构。

高阶 PDD 把观察对象从单个邻点扩大到多个邻点组成的点组。例如，二阶 PDD 考察一个中心点和两个邻点形成的三点组，并用三条边长的平均值打分。把所有点组的分数从小到大排列，就得到描述该中心点周围结构的序列。

本文研究两个相互关联的问题。随着考察的点组越来越多，排序靠后的分数按什么规律增长？如果只比较序列的前几列，能否提前判定两种结构在完整比较中也相距较远？

## 主要结果

### 1. 证明高阶极限猜想，给出显式系数

本文证明了 Widdowson 和 Kurlin 提出的猜想 5.5。对于 $n$ 维满秩周期点集，固定阶数 $h$ 后，以基元点 $p$ 为中心的第 $k$ 个排序得分满足

$$
a_p(h,k)=C_*(h!k)^{1/(nh)}+O(1).
$$

这个公式也适用于对所有基元点取平均后的得分。误差项 $O(1)$ 表示，当 $k$ 增大时，实际得分与主项的差始终有界。系数 $C_*$ 可以由晶体的点密度和一个范数单位球的体积算出。

证明的思路是把每一组邻点看成高维空间中的一个点。得分小于给定阈值，相当于这个点落在一个由得分函数定义的球内。先用球的体积估计点组数量，再由数量反推出第 $k$ 个得分。这个结果也说明，固定维数和阶数后，同密度晶体共享相同的主导增长项。区分这些晶体还需要考察有限位置上的得分及其差异。

### 2. 给出一个可以逐项验算的筛选反例

原论文提出用较短的偏差序列提前筛除相距较远的结构。本文发现，如果每次增加参与比较的列数时都重新拟合基线，这种筛选可能误删候选结构。

反例只用到两个一维周期点集 $\mathbb Z$ 和 $2\mathbb Z$。在论文所用的联合二阶距离下，取前 9 列得到的距离大于 $0.533$，取前 10 列得到的距离却不超过 $0.533$。如果以 $0.533$ 为阈值，9 列比较会将这一对结构提前筛除，而 10 列比较会将其保留。论文给出了可用有理数运算核对的证明。

### 3. 给出可靠筛选的条件，并改进扰动界

本文证明，在固定基线并使用未归一化的 $L_q$ 行代价时，短序列的运输距离是长序列距离的下界。若比较时需要重新拟合基线，采用未归一化的平方欧氏运输代价也能保留这一性质。因此，只要短序列给出的下界已经超过阈值，就可以提前排除这一对结构。

对满足论文配对条件的周期位移，如果每个点至多移动 $\varepsilon$，本文还将拟合后均方根运输距离的上界从 $4\varepsilon$ 改进到 $2\varepsilon$。

## 对晶体材料研究有什么意义？

以晶体数据库查重为例，一个新结构可能要与大量候选结构逐一比较。短序列筛选可以减少需要完整计算的候选对，但前提是提前排除的结构在完整比较中也确实超出阈值。本文的反例给出了可能误删候选的具体条件，筛选定理则说明怎样选择基线和代价，才能让这种提前排除有数学保证。

本文研究的是无标签周期点集的几何比较。定理给出的是筛选正确性和扰动稳定性的保证，实际能节省多少计算还取决于数据库和所选阈值。

## 论文下载

| 语言 | 阅读版 |
| --- | --- | --- | --- |
| 中文 | [PDF](crystal-pdd-zh.pdf) 
| English | [PDF](crystal-pdd-en.pdf) 

## Lean 4 形式化验证

主要结果已使用 Lean 4.28.0 和 Mathlib 检验，包括极限与有界误差、整数格点反例、前缀筛选不等式和扰动上界。[公理审计源码](pdd-lean-verification/PDDLimit/Audit.lean)集中列出了接受审计的 20 条定理。[本地检验记录](pdd-lean-check.txt)包含构建结果和公理列表。

下载或克隆本仓库后，可以在装有 Lean 和 Lake 的环境中运行：

```sh
cd pdd-lean-verification
lake build --wfail
```

## English summary

This paper proves Widdowson and Kurlin's Conjecture 5.5 on higher-order pointwise distance distributions, giving an explicit density-dependent limit and a bounded additive error. An exact counterexample on the integer lattices shows that refitting the baseline can invalidate short-prefix screening. The paper establishes conditions that recover a valid screening bound and improves the RMS perturbation estimate. The main results and the counterexample were checked in Lean 4.

## 参考论文

Daniel Widdowson and Vitaliy Kurlin, *Higher-order, generically complete,
continuous, and polynomial-time invariants of crystalline materials*,
Computational and Applied Mathematics 46, article 67 (2027).
[DOI: 10.1007/s40314-026-03905-z](https://doi.org/10.1007/s40314-026-03905-z)
