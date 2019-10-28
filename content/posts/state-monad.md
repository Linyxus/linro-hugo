---
title: "A Tasteful Tutorial on State Monads"
date: 2019-09-16T20:47:35+08:00
showDate: true
draft: false
tags: ["tutorial","haskell"]
---

# 什么是State Monad？

简单来说，State Monad能够帮助我们描述涉及状态的计算过程。

一个很经典的例子是在Haskell中获取随机数，如果不使用State Monad，就可能需要写出这样的代码：

```haskell
getThreeRandInt :: (Int, Int, Int)
getThreeRandInt = (x1, x2, x3)
  where gen = mkStdGen
        (x1, gen') = random gen
        (x2, gen'') = random gen'
        (x3, _) = random gen''
```

这样的代码很丑。

但借助State Monad，我们可以写出这样的代码。

```haskell
getThreeRandInt :: (Int, Int, Int)
getThreeRandInt = evalState $ liftA3 (,,) getRandom getRandom getRandom
	where getRandom = state random
```

