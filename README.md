# 晶体高阶逐点距离分布的渐近规律、筛选下界与扰动稳定性

*Higher-order crystal distance distributions: asymptotics, screening bounds, and perturbation stability*

**AI 作者（仓库贡献署名）** Codex GPT-6.2 Sol High  
**通讯作者** Wang Jian

论文研究如何用局部点组的距离比较周期晶体结构。我们证明了高阶距离得分的渐近规律，给出一个揭示短前缀筛选风险的精确反例，并建立了有数学保证的筛选下界和扰动界。数学推导、形式化与稿件起草使用了 Codex 协作。

## 论文与证明

| 语言 | 论文 PDF | LaTeX 正文 |
| --- | --- | --- |
| 中文 | [阅读中文版](crystal-pdd-zh.pdf) | [查看中文源码](crystal-pdd-zh.tex) |
| English | [Read the English paper](crystal-pdd-en.pdf) | [View the English source](crystal-pdd-en.tex) |

[Lean 4 工程 ZIP](pdd-lean-verification.zip) · [Lean 检验记录](pdd-lean-check.txt)

## 研究背景

同一晶体可以用不同的晶胞、原点和基底表示。逐点距离分布（Pointwise Distance Distribution，PDD）通过保留各基元点的近邻距离序列，为比较这些几何结构提供了一种统一方式。高阶 PDD 进一步比较邻点之间的距离。例如，二阶得分取一个中心点与两个邻点所组成三角形的平均边长，再按大小排列这些点组得分。

当排序位置不断后移，整体点密度会影响得分的增长速度。逐点偏差分布（Pointwise Deviation Distribution，PDA）通过扣除一条基线来比较剩余差异。本文研究这条基线怎样确定、只比较前几列时哪些排除决定有保证，以及小幅坐标扰动会使比较结果改变多少。

## 主要结果

### 1. 证明高阶极限，并求出二阶体积的解析式

对于 $n$ 维无标签满秩周期点集，固定阶数 $h$ 后，以基元点 $p$ 为中心的第 $k$ 小得分满足

```math
a_{p}(h,k)=C_{\ast}(h!k)^{1/(nh)}+O(1),
\qquad
C_{\ast}=(\rho^{h}V_{n,h})^{-1/(nh)}.
```

其中 $\rho$ 是点密度，$V_{n,h}$ 是乘积空间中得分范数的单位球体积。对各基元点取平均也满足同一公式，因此在上述范围内证明了 Widdowson 和 Kurlin 的猜想 5.5。$O(1)$ 表示与主项的差保持有界。一维格点的精确计算展示了归一化得分收敛时，加性误差仍可振荡。

证明先估计给定阈值以内的点组数量，再由计数反推出第 $k$ 小得分。对二阶情形，还可以固定一个邻点，把另一个邻点的允许区域写成椭球截面。对截面积分给出任意维数的解析式。在一、二、三维，二阶单位球体积分别为 $27/4$、$27\pi^2/14$ 和 $567\pi^2/160$。

### 2. 说明重新拟合为何会导致筛选误删

假设把短前缀距离当作长前缀距离的下界。只要短距离超过阈值，就会提前排除候选。这个规则需要保证拉长序列后距离不会跌到阈值以下。

对于整数格点 $\mathbb Z$ 和 $2\mathbb Z$，采用原论文的拟合基线及联合二阶 $L_\infty$ 距离时，九列和十列的结果满足

```math
D_{10}\le 0.533 < D_9.
```

九列的距离约为 $0.536715$，十列约为 $0.528232$。因此，以 $0.533$ 为阈值时，九列会排除这对结构，十列则会保留。严格比较由有理数证书给出。另一个一阶反例说明，即使基线固定，均方根归一化也能使距离随前缀增长而下降。

### 3. 建立可用的筛选下界，并改进扰动界

- **固定基线。** 使用未归一化的 $L_q$ 行代价时，短前缀的运输距离是长前缀距离的下界。使用均方根（RMS）行代价时，短距离需乘以 $\sqrt{k/K}$，其中 $k$ 和 $K$ 分别为短、长前缀长度。
- **逐长度重新拟合。** 采用未归一化的平方欧氏运输代价 $W_2^2$ 时，增加列数仍保持代价单调不减。这一结论为该平方代价提供筛选下界。
- **扰动稳定性。** 若两个周期点集具有共同的平移晶格，并能按平移规律一一配对，使每个点的位移至多为 $\varepsilon$，则拟合后 PDA 的 RMS 运输距离至多为 $2\varepsilon$。证明利用最小二乘基线对应的正交投影。

## 数值实验与应用范围

论文对一、二、三维格点的一阶和二阶得分计算到第 4096 位，并比较了同密度的二维方格与矩形格。筛选实验用 61 个缩放格点候选比较短下界与长前缀判定。扰动实验用交替挪动的一维周期点列展示实际计算值与理论上界的关系。

这些结果为晶体数据库去重和近邻检索提供了一个可证明的判断条件。若短前缀给出的下界已超过目标阈值，就可以在计算完整距离之前排除候选。当前实验使用合成几何结构。真实数据库上的筛选效率、近邻检索表现，以及含元素标签、缺陷或低秩周期性的结构比较，仍需进一步评估。

## Lean 4 验证与复现

工程采用 Lean 4.28.0 和 Mathlib 提交 `8f9d9cff6bd728b17a24e163c9402775d9e6a365`。已检查的核心结论包括高阶极限、有界加性误差、两个筛选反例及固定基线、平方运输和拟合 RMS 的相关界。20 条审计命题仅依赖 `propext`、`Classical.choice` 和 `Quot.sound`。二阶椭球截面公式在论文中以解析推导和数值积分核对，属于本次 Lean 审计之外的结果。

下载并解压 [Lean 4 工程](pdd-lean-verification.zip)。安装 Lean 工具链后，在包含 `lakefile.toml` 的目录中运行下面的命令。

~~~sh
lake build --wfail
lake env lean PDDLimit/Audit.lean
~~~

## LaTeX 编译说明

仓库中的两份 `.tex` 是重构稿正文。完整编译还需要共享的 `preamble.tex`、`references.bib`、`figures/` 和 `data/generated-tables.tex`。这些依赖随完整重构包提供。直接阅读请使用上方的两份 PDF。

## English summary

The paper proves the higher-order limit conjecture for full-rank unlabelled periodic point sets, with an explicit density-dependent coefficient and a bounded additive error. It evaluates the order-two norm-ball volume in every dimension. A certified counterexample shows that refitting the baseline can invalidate short-prefix screening. Fixed baselines and squared Euclidean transport recover valid screening bounds. A coupling and projection argument gives a fitted RMS perturbation bound of $2\varepsilon$ under the stated matching condition. The principal results and the counterexamples were checked in Lean 4.

## 参考论文

Daniel Widdowson and Vitaliy Kurlin. *Higher-order, generically complete, continuous, and polynomial-time invariants of crystalline materials*. **Computational and Applied Mathematics** 46, article 67 (2027). [出版社页面](https://link.springer.com/article/10.1007/s40314-026-03905-z)
