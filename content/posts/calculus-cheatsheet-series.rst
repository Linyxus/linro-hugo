---
title: "Calculus Cheatsheets :: 级数"
date: 2019-05-02T19:09:04+08:00
draft: false
showDate: True
tags: ["note", "calculus"]
---

常数项级数
==========

概念
----

定义
++++

各项均为常数的级数

部分和
++++++

.. math::

   \sigma (n) = \sum_{k=0}^{n} a_k

敛散性
++++++

若

.. math::

   lim_{n \rightarrow \infty} \sigma (n) = A 存在

则称级数收敛于 :math:`A` 。

否则称级数发散。

收敛级数性质
------------

1. 线性性质

2. 归并性质

正项级数审敛法
--------------

比较审敛法
++++++++++

.. math::

   y_n \le x_n \wedge x_n收敛 \Rightarrow y_n 收敛 \\
   y_n \ge x_n \wedge x_n发散 \Rightarrow y_n 发散

极限形式：

.. math::

   \lim_{n \rightarrow \infty} \frac {a_n} {b_n} = +\infty \Rightarrow b_n 发散则 a_n 发散 \\
   \lim_{n \rightarrow \infty} \frac {a_n} {b_n} = 0 \Rightarrow a_n 收敛则 b_n 收敛 \\
   \lim_{n \rightarrow \infty} \frac {a_n} {b_n} = k (k > 0) \Rightarrow a_n, b_n 同时敛散

比值审敛法
++++++++++

.. math::

   \lim_{n \rightarrow \infty} \frac {a_{n+1}} {a_n} = K

- 若 :math:`K < 1` 则 :math:`a_n` 收敛

- 若 :math:`K > 1` 则 :math:`a_n` 发散

- 若 :math:`K = 1` 则 :math:`a_n` 敛散性未知

根式审敛法
++++++++++

.. math::

   \lim_{n \rightarrow \infty} \sqrt[n]{a_n} = \lambda

情况可类比比值审敛法。

任意项级数审敛法
----------------

思想是转化为正项级数，然后利用正项级数审敛法。

交错级数判别法
++++++++++++++

若级数 :math:`\sum{n=1}{\infty} (-1)^n a_n` 满足：

- :math:`\lim_{n \to \infty} a_n = 0`

- :math:`a_n` 单调递减

则级数收敛。

绝对收敛与条件收敛
++++++++++++++++++

定义
````
:绝对收敛: 每一项取绝对值后仍然收敛

:条件收敛: 每一项取绝对值后发散，原级数收敛

绝对收敛级数的性质
``````````````````

重排后仍然收敛到同一个值。

函数项级数
==========

傅里叶级数
----------

对于周期 :math:`T = 2l` 的函数 :math:`f(x)` ，其傅里叶级数为：

.. math::

   f(x) = a_0 + \sum_{n=1}^{\infty} a_n cos(\frac{n \pi x}{l}) + \sum_{n=1}^{\infty} b_n sin(\frac{n \pi x}{l})

其中，

.. math::

   a_0 &= \frac {1} {2l} \int_{-l}^{l} f(x) dx \\
   a_n &= \frac {1} {l} \int_{-l}^{l} f(x) cos \frac {n \pi x} {l} dx \\
   b_n &= \frac {1} {l} \int_{-l}^{l} f(x) sin \frac {n \pi x} {l} dx


:math:`f(x)` 的奇偶性简化运算
-----------------------------

1. :math:`f(x)` 为奇函数，则 :math:`b_n=0`

2. 偶函数相应同样成立

