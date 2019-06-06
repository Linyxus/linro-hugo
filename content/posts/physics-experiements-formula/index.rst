---
title: "物理实验数据处理公式总结"
date: 2019-05-10T13:12:24+08:00
draft: false
showDate: true
tags: ["note", "physics"]
---

{{< figure src="mindmap.png" >}}

贝塞尔公式
==========

.. math::

   s(x) = \sqrt {\frac{\sum (x_i - \bar{x})^2} {n-1}}

.. math::

   s(\bar{x}) = \frac {s(x)} {\sqrt{n}} = \sqrt {\frac{\sum (x_i - \bar{x})^2} {n(n-1)}}

直接测量量不确定度
==================

当 :math:`n=6` 时，

.. math::

   u_a(x) = t_ps(\bar{x}) = s(x)

且有

.. math::

   u_b(x) = \Delta_{仪}

合成不确定度：

.. math::

   u(x) = \sqrt{u_a^2(x) + u_b^2(x)}

间接测量量不确定度
==================

1. 全微分 -> 绝对

2. 取对数后全微分 -> 相对


误差等分配原则
==============

也即合成不确定度中各平方项相等。

最小二乘法
==========

如果设：

.. math::

   y = kx + b

可以求解出：

.. math::

   k &= \frac {\bar{xy} - \bar{x} \bar{y}}{\bar{x^2} - \bar{x}^2} \\
   b &= \bar{y} - k \bar{x}

相关系数：

.. math::

   r = \frac {\bar{xy} - \bar x \bar y} {\sqrt {(\bar{x}^2 - \bar {x^2}) (\bar{y}^2 - \bar{y^2})}}
