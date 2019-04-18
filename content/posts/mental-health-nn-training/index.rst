---
title: "雏雁计划神经网络训练记录"
date: 2019-04-09T15:40:08+08:00
draft: false
showDate: true
tags: ["machine learning", "note"]
---


思路
====

| 使用前20个问题预测剩下11个问题的答案。将经常、有时、很少、从不规范化为[1, 0.33, -0.33, -1]。


训练笔记
========


Small ReLU
----------

20 -> 18 -> 11

*Result:*

- acc :math:`65.22 \%`

- test_acc :math:`45.45\%`


Mid Relu
--------

20 -> 30 -> 11

*Result:*

- acc: 0.7521

- test_acc: 0.4727

- test_acc_l: 0.6636


Tiny Relu
---------

20 -> 10 -> 11

*Result:*

- acc: 0.5513

- test_acc: 0.5091

- test_acc_l: 0.5509


Smaller Relu
------------

20 -> 15 -> 11

*Result:*

- acc: 0.5853

- test_acc: 0.5236

- test_acc_l: 0.7909


Smaller Relu V2
---------------

Difference: change batch_size to 32, shuffle data.

20 -> 15 -> 11

*Result:*

- acc: 0.5712

- test_acc: 0.5109

- test_acc_l: 0.76

Mid Relu V2
-----------

20 -> 28 -> 11

*Result:*
