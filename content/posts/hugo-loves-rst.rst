---
title: "Hugo Loves Rst"
date: 2019-04-04T12:54:40+08:00
draft: true
showDate: true
tags: ["hello"]
---

==========
Main Title
==========

This is a title.
================

This is a subtitle, and some math.
----------------------------------

.. math::
   dz = \frac{\partial z}{\partial x}dx + \frac{\partial z}{\partial y}dy

.. math::
   \begin{bmatrix}
   x_{11} & x_{12} & x_{13} & \dots  & x_{1n} \\
   x_{21} & x_{22} & x_{23} & \dots  & x_{2n} \\
   \vdots & \vdots & \vdots & \ddots & \vdots \\
   x_{d1} & x_{d2} & x_{d3} & \dots  & x_{dn}
   \end{bmatrix}

.. math::
   \max{|x_k^{(1)}-a^{(1)}|, …,|x_k^{(n)}-a^{(n)}|} \leq ||X_k - A|| \leq \sum{i=1}^k |x_k^{(i)}-a^{(i)}|

Some lists
==========

but some text first
-------------------

Some text.

*Italic text*

**Bold text**

``code``

and here are the lists
----------------------

- Item
- Item 2

  - Item 2.1
  - Item 2.2

different list types:

1. Item 1

2. Item 2

   1. subitem
   2. subitem again

or some chinese
===============

啦啦啦啦啦啦啦啦

Code is also okay!
==================

.. code:: python

   def my_function():
     "just a test"
     print 8/2

or with line numbers

.. code:: c
   :number-lines: 1

   #include <iostream>
   using namespace std;

   int main () {
     return 0;
   }

Sometimes tables are useful
===========================

.. table:: Truth table for ``not``
   :widths: auto

   ===== =======
     A    not a
   ===== =======
   True  False
   False True
   ===== =======
