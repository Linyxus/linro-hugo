+++
title = "The magic of Laziness: from fibonacci to Y combinator (1)"
author = ["Yichen Xu"]
date = 2020-02-12T21:17:00+08:00
tags = ["tutorial", "haskell"]
draft = false
showDate = true
+++

在这篇博客中，我将简单地介绍许多函数式编程语言中一个重要的，也是独特的特性：Laziness。


## A first taste of Laziness {#a-first-taste-of-laziness}

所以，什么是Laziness呢？Laziness有一个更加正式的名称：lazy evaluation，也即 _惰性求值_ 。简单来说，惰性求值意味着，表达式的值不会被立即求出，只有当需要的时候才会对其进行求值。

比如，下面的代码在运行时并不会报错，而可以安全地求出列表的长度。

```haskell
length [error "Hey!", error "Here is an error."]
-- 2
```

原因是，在 `length` 的实现中，并不关心列表中每个元素的值是什么。因而他们并不会被求值。

```haskell
length :: [a] -> Int
length [] = 0
length (_:xs) = 1 + length xs
```

这是惰性求值这一特性最为直接的展示。

而在实现上，具有惰性求值特性的代码可以这样被直观地想象：所有的值都被装在一个盒子中，对某个值求值的过程，也就是打开盒子的过程，为了打开这个盒子之后或许我们会发现里面有更多的盒子要打开。而所有的盒子，都会在万不得已的时候才被打开。

这个比喻很直观，但不那么确切。比较精确的说法应当基于两个定义（至少在Haskell中是如此）：Normal Form与Weak Head Normal Form。

Normal Form的定义很简单：normal form表示一个被完全求值的值：代表它所有的部分都已经被完全地求值完毕。 `1` ， `Just "Hello"` ，都是normal form，而 `1 + 1` 则不是。

Weak Head Normal Formd的定义则要复杂一些。简单来说，所有函数都是WHNF。所有顶层
constructor被求值的表达式也都是WHNF。

举一些简单的例子： `(1, 2 + 2)` ， `(1 + 1) : undefined` ， `Just 0` 都是WHNF，因为他们的顶层constructor都已求值。

而一般来说，对某个值进行求值的时候，都只会将它求值为WHNF的形式。

更为详细的介绍，可以在这一篇 [文章](https://alpmestan.com/posts/2013-10-02-oh-my-laziness.html) 中找到。

Laziness经常被认为会导致更多的内存占用，大多数时候的确如此：因为Laziness很多时候会导致运算过程被保存，而不是直接求出结果。但有些时候，Laziness可以带来更高的内存使用效率，并做到若没有Laziness则完全不可能做到的事情，举一个简单的例子。

```haskell
x :: [Int]
x = 1 : x
```

上面的函数定义了一个只包含 \\(1\\) 的无穷序列。这在非Lazy的语言中是不可能做到的：它会消耗无穷的内存。而在Lazy的语言中，它在内存中的形态不过是一个循环链表。


## 利用惰性定义无穷序列 {#利用惰性定义无穷序列}

那么，惰性到底可以做什么有用的事情呢？

下面这段代码定义了所有自然数的序列：

```haskell
nat :: [Int]
nat = 0 : map (+1) nat

-- >>> take 3 nat
-- [0,1,2]
```

或者，可以类似地定义等比数列 $\\{ 2^n \\}<sub>n &isin; \mathbb N</sub>$：

```haskell
xs :: [Int]
xs = 1 : map (^2) nat
```

正是惰性求值的特性，给予了我们定义无穷数列的能力。考虑下面的代码：

```haskell
take 3 nat
-- take 3 (0 : map (+1) nat)
-- 0 : take 2 (map (+1) nat)
-- 0 : take 2 (map (+1) (0 : map (+1) nat))
-- 0 : take 2 ((0 + 1) : map (+1) (map (+1) nat))
-- 0 : (0 + 1) : take 1 (map (+1) (map (+1) nat))
-- ...
-- 0 : (0 + 1) : (0 + 1 + 1) : []
-- [0,1,2]

take :: Int -> [a] -> [a]
take n xs | n <= 0 = []
take _ [] = []
take n (x:xs) = x : take (n-1) xs
```

这段代码非常简单地解释了惰性求值的过程。值得注意的是：

-   在上面的代码中，每次对列表求值时，都仅仅求值到WHNF，也即 `x : xs` 的形式。
-   `take` 中的比较 \\(n \le 0\\) 将会强制将两端都求值到NF。

上面的定义基于了这样一个事实：所有带有符合递推式 $ a<sub>k+1</sub> = f(a\_k), $
且首项 \\(a\_0\\) 为 \\(c\\) 的无穷序列，都满足这一恒等式：

```haskell
xs === c : map f xs
```

因此我们可以对所有一阶递推序列都在Haskell中非常容易地构造出对应的无穷序列。

而对于二阶递推式也是类似的。因为我们有

```haskell
xs === x0 : x1 : zipWith f xs (drop 1 xs)
```

其中 `f` 即为二阶递推关系式。

因而可以很容易地定义出Fibonacci数列：

```haskell
fibo :: [Int]
fibo = 1 : 1 : zipWith (+) fibo (drop 1 fibo)
```

而类似定义的这些函数，在不具备Lazy特性的语言中是没有任何用的。调用他们只会进入死循环。

当然，归功于Laziness，我们也能对无穷序列进行很容易的操作。例如，如果你想求出
Fibonacci数列两项之差所构成的数列（这显然也是一个Fibonacci数列），可以这样写：

```haskell
xs = zipWith subtract fibo (drop 1 fibo)
```

`xs` 也将是一个无穷序列。

在下一篇博客中，我将介绍一个非常有趣的情景：假设我们正在使用一个非常，非常，非常简单的纯函数语言，不仅没有任何副作用，并且简单到无法定义变量：所有名称的引入都是通过定义匿名函数完成的。而整个程序也仅仅是一个表达式而已。我们用简单的Haskell来模拟这种语言。也即：我们只能使用Haskell中的Lambda函数，不能定义变量。

```haskell
(\x y -> x + y) 1 2
-- 3
```

而在这一情景下，该如何实现递归呢？也即，如果无法定义一个名称，那函数该如何“引用”自己呢？

为了在这种情况下实现递归，我会介绍Y组合子的概念，并指出，是Laziness使Y组合子的定义成为可能。
