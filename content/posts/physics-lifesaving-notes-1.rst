---
title: "Physics Lifesaving Notes :: 质点运动学基础"
date: 2019-04-18T20:37:30+08:00
draft: false
showDate: true
tags: ["note", "physics"]
---

基本概念
========

:质点:
   大小、形状可以忽略的物体
:质点系:
   多个质点的组合

基本参数
========

位置矢量
--------

.. math::

   \vec{r} = x \vec{i} + y \vec{j} + z \vec{k}

运动方程
--------

.. math::

   \vec{r} = \vec{r}(t)


轨迹方程
--------

将运动方程中的 :math:`t` 消去即得到轨迹方程。

位移与路程
----------

位移为始末位置矢量的改变量。

.. math::

   \Delta \vec{r} = \vec{r}_2 - \vec{r}_1

路程为运动轨迹的长度。

位移与路程的关系
~~~~~~~~~~~~~~~~

1. 位移不等于路程：位移是矢量，路程是标量

2. :math:`\lim_{\Delta{t} \rightarrow 0} |\Delta \vec r| = \Delta s` ，也即 :math:`|dr| = ds`

速度
----

.. math::

   \vec{v} = \frac{d \vec{r}}{dt}

加速度
------

.. math::

   \vec{a} = \frac{d \vec{v}} {dt}


自然坐标系
==========

已知质点运动的轨迹：

:math:`\vec{e}_n` 法向单位矢量，指向轨迹曲线 **内侧**

:math:`\vec{e}_{\tau}` 切向单位矢量，指向自然坐标正向

不是恒矢量，会随着质点的位置改变方向。

故有

.. math::

   \vec{v} = v \vec{e}_{\tau}

圆周运动的角量参数
==================

:角位置:
   .. math::
      \theta = \theta(t)

:角速度:
   .. math::
      \omega = \frac{d \theta} {dt}

:角加速度:
   .. math::
      \beta = \frac {d \omega} {dt}

角量与线量之间的关系如下

.. math::

   ds = R d\theta

.. math::

   v = R \omega

.. math::

   a_{\tau} = R \beta

.. math::

   a_{t} = R \omega^2


参考系与相对运动
================

惯性参考系
----------

做匀速运动的参考系为惯性参考系。

伽利略速度变换公式

.. math::

   \vec{v} = \vec{v}_0 + \vec{v}^{'}


非惯性参考系
------------

伽利略速度变换公式仍然适用，但物体将受到惯性力

.. math::

   \vec{F} = - m \vec{a}_0

在转动非惯性参考系中，形式改变为惯性离心力

.. math::

   \vec{F} = - m \omega^2 \vec{R}
