---
title: "Physics Lifesaving Notes :: 静电场中的导体与电介质"
date: 2019-04-08T19:46:14+08:00
draft: false
showDate: true
tags: ["note", "physics"]
---

静电平衡
========

导体的定义
----------

1. 具有大量能够自由移动的电子

2. 在外电场的作用下，能够很好的传导电流

的物质。

静电平衡的条件与特征
--------------------

| 条件：

1. 导体内部任意一点的电场强度都为0

2. 导体表面上任意一点的电场强度方向垂直于该出导体表面

| 除此以外，由于导体内部电场处处为0，由于

.. math::

   E = - \frac{\partial V}{\partial l}

| 因而电势处处相等，导体成为等势体。

静电平衡的理解
--------------

| 与 *霍尔效应* 有几分类似。导体内部的自由电荷会均衡它收到的电场，内部电荷产生的电场与外加电场抵消，
  内部电场处处为0，导体达到静电平衡。

静电平衡时的电荷分布
--------------------

电荷分布
~~~~~~~~

| 处于静电平衡状态的导体，内部处处没有净电荷存在，电荷只分布在导体表面上。

| 证明方法很简单，用高斯定理。

| 当到体内有空腔时，空腔表面也无电荷。证明如下：

| 取包围空腔的高斯面，由于电场强度 :math:`E` 处处为 :math:`0`，故空腔表面净电荷也为零。假设空腔表面一部分
  正电荷，一部分带负电荷，则空腔内存在电场。故而电势沿电场线方向有上升或降落，而静电场为保守场切导体内部处处
  等电势，故而矛盾。故不存在电荷。

贴近表面处的电场强度
~~~~~~~~~~~~~~~~~~~~

.. math::

   E = \frac{\sigma}{\epsilon_0}


| 使用高斯定理证明即可。

| 值得一提的是，面积元 :math:`\Delta S` 事实上可以等效为无限大带点平面，它对靠近表面的点 :math:`P` 产生的
  场强事实上为

.. math::

   E_1 = \frac{\sigma}{2\epsilon_0}

| 而余下所有电荷在此处产生的场强和为

.. math::

   E_2 = \frac{\sigma}{2\epsilon_0}

| 才有

.. math::

   E = E_1 + E_2 = \frac{\sigma}{\epsilon_0}

| 因而导体表面场强事实上为

.. math::

   E^{'} = E_2 = \frac{\sigma}{2\epsilon_0}

静电感应与静电屏蔽
------------------

静电感应
~~~~~~~~

| 导体在外加电场 :math:`E_0` 的影响下，被感应出感应电荷，达到静电平衡状态。

| 原理为：在电场 :math:`E_0` 的作用下，导体内的电子产生移动，在导体的一面积累而呈现带负电荷，另一面由于
  缺少电子而呈现正电荷。

| 于是导体内产生感应电场 :math:`E^{'}` ，当感应电场与外加电场抵消也即 :math:`E_0 + E^{'} = 0` 时，达到
  静电平衡。

静电屏蔽
~~~~~~~~

| 因此空腔导体具有静电屏蔽作用。

1. 空腔导体内无电荷。

2. 空腔导体内有电荷。

| 空腔内壁上会被感应出等量异号电荷，因此表面也会产生等量同号电荷。这时将导体外壁接地，则被导体包裹的电荷将不能
  对外界产生任何影响。

*例题*
  两块等面积金属板，分别带电荷 :math:`q_A` 和 :math:`q_B` ，平板面积为 :math:`S` ，两板间距为 :math:`d`
  。且面积线度远大于间距。试求静电平衡时两板各表面上的电荷面密度。

*解析*
  根据电荷守恒与板内电场强度为0列出方程即可求解。

  .. math::

    \begin{cases}
      \sigma_1 = \sigma_4 = \frac{q_A + q_B}{2S} \\
      \sigma_2 = -\sigma_3 = \frac{q_A - q_B}{2S}
    \end{cases}

静电场中的电介质
================

电介质的极化
------------

*与导体类似* ，电介质在电场中也会因相似的原理极化。

与导体的区别在于，电介质内部会产生感应电场抵消外加磁场，但是不能完全抵消，内部电场不为零。

相对电容率
----------

当电介质被放入平行板之间时，电势差 :math:`U_0` 减少为 :math:`U` 。设：

.. math::

  U = \frac {U_0} {\varepsilon_r}

其中 :math:`\varepsilon_r` 即为相对电容率，为大于 :math:`1` 的纯数。

而又由于平行板之间的电场为匀强电场，故：

.. math::

   U_0 = E_0 d \\
   U = E d

于是有：

.. math::

   E = \frac {E_0} {\varepsilon_r}

定义极化电荷面密度为：

.. math::

   \sigma^{'} = \frac{E^{'}}{S}

可以推出：

.. math::

   \sigma^{'} = (1-\frac{1}{\varepsilon_r}) \sigma_0

可以看到，:math:`\varepsilon_r` 越大，极化电荷面密度越接近原电荷密度，对原电场的抵消就越大。

电位移
------

.. math::

   \oint_S E \cdot dS = \frac{q_0}{\varepsilon_0 \varepsilon_r}

故而定义：

.. math::

   D = \varepsilon_0 \varepsilon_r E

为 **电位移** 。

这个矢量没有很多的实际意义，只是为了方便计算，如此定义之后，高斯定理即可写成：

.. math::

   \oint_S D \cdot S = q_0

电容
====

定义
----

.. math::

   C = \frac {q} {V}

常见形式的电容
--------------

1. 导体球

.. math::

   V & = \frac{q} {4 \pi \varepsilon_0 R} \\
   C & = \frac{q} {V} = 4 \pi \varepsilon_0 R

2. 电容器

.. math::

   E &= \frac {\sigma}{\varepsilon_0 \varepsilon_r} \\
   U &= Ed = \frac {\sigma d} {\varepsilon_0 \varepsilon_r} \\
   C &= \frac{q}{U} = \frac{\varepsilon_0 \varepsilon_r S}{d}

3. 球形电容器

由两个半径分别为 :math:`R_1` 与 :math:`R_2` 的同心金属球壳组成。带电量分别为 :math:`+q` 与 :math:`-q` 。

可知两球面间的电场强度为

.. math::

   E = \frac {q} {4 \pi \varepsilon_0 \varepsilon_r r^2}

积分，可得电势差

.. math::

   U = \int_{R_1}^{R_2} E \cdot dr = \frac {q} {4 \pi \varepsilon_0 \varepsilon_r}
   \frac {R_2 - R_1} {R_1 R_2}

故而

.. math::

   C = \frac{q}{U} = \frac {4 \pi \varepsilon_0 \varepsilon_r R_1 R_2} {R_2 - R_1}

讨论两个情况：

(1) 假设 :math:`R_1` 和 :math:`R_2` 都非常大，而 :math:`d=R_2-R_1` 很小，则：

    .. math::

       R_1 R_2 = R_1^2 \\
       S = 4 \pi R_1^2

    得到：

    .. math::

       C = \frac {\varepsilon_0 \varepsilon_r S} {d}

    也即平行板电容器的表达式。

(2) 若 :math:`R_2 \gg R_1`

    .. math::

       C = 4 \pi \varepsilon_0 \varepsilon_r R_1

    即为孤立导体球电容表达式。


4. 同轴电缆

应用高斯定理，积分求电势差，类似可得

.. math::

   C = \frac {2 \pi \varepsilon_0 \varepsilon_r L} {\ln \frac {R_b} {R_a}}

电场能量
========

平行板电容器之间的电压为

.. math::

   U = \frac {q}{C}

将元电荷 :math:`d q` 从一极移动到另一极，需要做功

.. math::

   dA = U \cdot dq = \frac{q}{C} dq

故

.. math::

   A = \int_q \frac {q}{C} dq = \frac {Q^2}{2C}

故电场储能

.. math::

   W_e = \frac {Q^2}{2C} = \frac{1}{2} C U^2 = \frac{1}{2} QU

又有

.. math::

   U = Ed, C = \frac{\varepsilon S} {d}

可以推出

.. math::

   W_e = \frac{1}{2} \varepsilon E^2 \cdot Sd = \frac{1}{2} \varepsilon E^2 \cdot V

其中 :math:`V` 为电容器中电场遍及的空间体积。

于是可以定义 **电场能量密度**

.. math::

   w_e = \frac {W_e} {V} = \frac{1}{2} \varepsilon E^2

在引入电场强度密度之后，在不均匀电场中，可有：

.. math::

   dW_e = w_e dV

于是

.. math::

   W_e = \int_V dW_e = \int_V \frac{1}{2} \varepsilon E^2 dV .
