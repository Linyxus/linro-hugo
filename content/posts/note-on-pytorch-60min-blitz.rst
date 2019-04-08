---
title: "Note on Pytorch 60-min Blitz"
date: 2019-04-04T22:59:05+08:00
draft: false
showDate: true
tags: ["note", "pytorch"]
---

Tensor
======

Create a tensor
---------------

1. via torch.empty

.. code:: python

   x = torch.empty(5, 3)

| This will create a tensor with a size of (5, 3). It is not initialized.

2. via torch.zero
