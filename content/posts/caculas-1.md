---
title: "微积分笔记 #1 :: 多元微积分初步"
date: 2019-04-01T16:28:15+08:00
showDate: true
tags: ["caculus", "note"]
---

## n-dimension Euclid Space $R^n$

### 邻域

$$
X_0点的\delta邻域：B(X_0, \delta) = \{X \in R^n : ||X-X_0<\delta||\}
$$

$$
X_0点的去心\delta邻域：B(X_0, \delta) = \{X \in R^n : 0 < ||X-X_0|| < \delta\}
$$

### 点与集

**内点：**
$$
I(x) : x为\Omega的内点
$$

$$
I(x) \Leftrightarrow \exists \delta > 0, B(x,\delta) \subset \Omega
$$

**边界点：**
$$
S(x) \Leftrightarrow \forall \delta > 0 (B(x,\delta) \cap \Omega \neq \emptyset \wedge B(x,\delta) \cap R^n /\Omega \neq \emptyset)
$$
说明点$x$的任意一个邻域都横跨了集合$\Omega$的边界，很漂亮的定义。

**开集**：所有点都为内点

**闭集**：余集为开集

**内部**：集合所有内点构成的集合，$\dot{\Omega}$

**边界**：所有边界点，$\partial \Omega$

**闭包**：$\bar\Omega = \Omega \cup \partial \Omega$

### 点列的收敛性

### 定理

$$
\lim_{k \to +\infin} X_k = A \Leftrightarrow \lim_{k \to +\infin} x_k^{(i)} = a(i), i=1,2,...,n.
$$

#### 证明：

基于不等式：
$$
\max\{|x_k^{(1)}-a^{(1)}|, ...,|x_k^{(n)}-a^{(n)}|\} \leq ||X_k -A|| \leq \sum_{i=1}^k |x_k^{(i)}-a^{(i)}|
$$
再结合定义即可证明。

### Cauchy序列

$$
{X_k}为R^n中的Cauchy数列 \Leftrightarrow \forall \epsilon > 0, \exists N \in N^+, \forall l, m > N, ||X_l - X_m|| < \epsilon
$$

## n元函数与n元向量值函数

### n元函数

$$
f: \Omega \subset R^n \rightarrow R^1 \\
X \mapsto y
$$

从向量映射到标量。

### 向量值函数

$$
f: \Omega \subset \R^n \to \R^m \\
X \mapsto Y
$$

从向量映射到向量。

两个空间之间的变换。

## 多元函数的极限与连续

### 极限

先定义向量值函数极限：

若有
$$
f: \Omega \subset R^n \to R^m
$$

$$
\lim_{X \to X_0} f(X) = A \Leftrightarrow \forall \epsilon > 0, \exists \delta > 0, \forall X \in \Omega: 0 < ||X-X_0|| < \delta, ||f(X) - A||_m < \epsilon
$$

当$m=1$时，即为多元函数情况。

#### 多元函数极限的复杂性

极限路径：

- 一元函数：左右极限都存在 -> 存在

- 多元函数：无数条趋向路径

#### 边界点极限

$X_0$为边界点时的特殊情况。

称为当$X$在$\Omega$内趋近于$X_0$时的极限。

#### 运算法则

满足：加减乘除。

#### Cauchy准则

$$
\forall \epsilon > 0, \exists \delta > 0, \forall X', X'' \in B^{o}(X_0, \delta), ||f(X')-f(X'')|| < \epsilon
$$

#### 定理

海涅定理。
$$
\lim_{X \to X_0}f(X) = A \Leftrightarrow \forall \{X_k\}: \lim_{k \to \infin} X_k = X_0,\lim_{k \to \infin} f(X_k) = A
$$

#### 累次极限

上述极限被称为重极限。

累次极限即为先后求极限：
$$
\lim_{x \to x_0} \lim_{y \to y_0} f(x, y) = \lim_{x \to x_0} (\lim_{y \to y_0} f(x,y))
$$
先$y$后$x$也一样。

#### 二重极限与累次极限的关系

1. 二重极限与累次极限都存在，则三者相等。
2. 累次极限两个都存在但不等，则二重极限不存在。

### 连续

#### 定义

$$
\lim_{X \to X_0} f(X) = f(X_0)
$$

称$f(X)$在点$X_0$处连续。

对于边界点，使用边界极限的定义。

#### 性质

与一元连续函数完全类似。

1. 最值定理
2. 介值定理

## 多元函数的全微分

### 定义

类似于一元函数。
$$
设有X_0为\Omega内点，则\exist\Delta X:X+\Delta X \in \Omega，若\exist a_1 \Delta x_1 + a_2 \Delta x_2 + \cdots + a_n \Delta x_n :\\
\Delta u = f(X+\Delta x)-f(X) = a_1 \Delta x_1 + \cdots a_n \Delta x_n + o(\Delta X) (\Delta X \to 0) \\
则称f在X_0点可微。
$$

$$
du = a_1 dx_1 + a_2dx_2 + \cdots + a_ndx_n
$$

### 性质

1. 在$X_0$处可微的函数必在$X_0$处连续。
2. 可微函数的微分是唯一的。

证明2：

用反证法。

假设有：
$$
du = a_1dx_1 + a_2dx_2 + \cdots + a_ndx_n = b_1dx_1 + b_2dx_2 + \cdots + b_ndx_n
$$

$$
\Leftrightarrow (a_1, a_2, \cdots, a_n)\Delta{X} + o_1(\rho) = (b_1, b_2, \cdots, b_n)\Delta{X} + o_2(\rho)
$$

其中
$$
\rho = ||X||
$$
记：
$$
o_1(\rho) = \varphi(\Delta{X})||\Delta{X}||
$$

$$
o_2(\rho) = \psi(\Delta{X})||\Delta{X}||
$$

令$\Delta{X} = t\vec{e}，\vec{e}$为任意单位向量，代入即可得：
$$
(a_1-b_1, a_2-b_2, \cdots, a_n-b_n) = \vec{0}
$$
则得证。





