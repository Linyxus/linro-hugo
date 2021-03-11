---
title: "离散数学复习笔记 :: 逻辑学"
date: 2019-04-22T19:09:35+08:00
draft: false
showDate: true
tags: ["discrete math", "note"]
---

Proposition Logic Basics
========================

What is a proposition
---------------------

A statement of proposition is a declarative sentence that is
either true or false, but not both.


:The moon is made of green cheese:
   a proposition
:Go to town!:
   not a proposition
:What time is it?:
   not a proposition


Build up a proposition
----------------------

1. propostional variables

2. logical connectives


Kinds of logical connectives
----------------------------

1. Negation 'NOT' :: :math:`\neg`

2. Conjunction 'AND' :: :math:`\wedge`

3. Disjunction

   - Inclusive 'OR' :: :math:`\vee` (or)

   - Exclusive 'OR' :: :math:`\oplus` (xor)

4. Implication :: :math:`\rightarrow`

   Equivalent forms:

   - if p then q
   
   - if p, q
   
   - p implies q
   
   - p only if q
   
   - p is a sufficient condition for q
   
   - q when p
   
   - q follows from p
   
   - q whenever p
   
   - q unless not p
   
   - q is a necessary condition for p

   Transformations:
     
   - inverse: :math:`\neg p \rightarrow \neg q`

   - converse: :math:`q \rightarrow p`

   - contrapositive: :math:`\neg q \rightarrow \neg p`

5. Biconditional or equivalence :: :math:`\leftrightarrow`

   if and only if.

Precedence of logical connectives
---------------------------------

.. math::

   \neg > (\vee, \wedge) > (\rightarrow, \leftrightarrow)


Proposition Equivalence
=======================

Truth table
-----------

Example:

The truth table of :math:`\vee`:

=====  =====  ===============
  p      q    p :math:`\vee` q
=====  =====  ===============
False  False  False
True   False  True
False  True   True
True   True   True
=====  =====  ===============

Logical equivalence
~~~~~~~~~~~~~~~~~~~

The two propositions whose truth tables are the same are called equivalent.

Variant and truth tables
~~~~~~~~~~~~~~~~~~~~~~~~

If we have :math:`n` variables.

:math:`2^n` rows

:math:`2^{2^n}` different tables


Different kinds of propositions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Tautology : always true

2. Contradiction : always false

3. Contingency

Common logical equivalence
--------------------------
