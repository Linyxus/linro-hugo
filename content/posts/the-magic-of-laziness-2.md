+++
title = "The magic of Laziness: from fibonacci to Y combinator (2)"
author = ["Yichen Xu"]
date = 2020-03-22T10:35:00+08:00
tags = ["tutorial", "haskell"]
draft = false
showDate = true
+++

这是 The magic of Laziness 的第二篇博客。在本篇文章中，我将展示Laziness的另一个
奇妙的应用：创建递归函数。

创建递归函数自然是一件非常简单的事情，在Python中实现一个计算阶乘的函数，只需寥寥
数行：

```python
def fact(n: int):
    ret = 1
    for i in range(1, n+1):
        ret *= i
    return ret
```

事实上，使用函数式风格的语言来实现它，也非常容易：

```haskell
fact n | n <= 0 = 1 | otherwise = n * fact (n-1)
```

然而，今天我想解决的问题是，如何在一个定义极度简单，甚至无法为自定义的函数绑定
“名称”的条件下，实现递归？

确切来说，我们定义这样的一种简单的玩具语言，其中只有这样的几种简单的元素：

-   自然数字面值：\\(0, 1, 2, \cdots\\)
-   变量：\\(x, y, z, a, b, c, \cdots\\)
-   必要的自然数算符：\\(x + y, x \* 2, 2^{10}, \cdots\\)
-   函数表达式：\\(\lambda x. x+1\\)
-   应用函数表达式 (apply function)：\\((\lambda x. x+1) 0\\) (结果为\\(1\\))，可以理解为
    函数的“计算”，等价于将函数参数在函数体中所有的出现替换为对应的值产生的表达式。

更加正确地来说，该语言的文法可以如下定义：

```nil
nat ::= (0..9)+
var ::= (a..z) (a..z | 0..9)*
op ::= + | * | ^

bin_expr ::= expr op expr
lam_expr ::= \ var . expr
app_expr ::= expr expr
expr ::= bin_expr | lam_expr | app_expr | (expr) | nat | var
```

事实上，这样一种语言其实只是定义了一种书写 _表达式_ 的方式。而基于本语言的计算，
事实上也只是对表达式的化简过程。

那么，回到最初的问题，如何在这样的一个语言中实现递归呢？所谓递归，就是在函数中调
用自身，然而这个语言中根本就没有办法给函数定义一个“名字”，更不用说调用自己了。

在正式解决解决这个问题之前，需要先做一些准备工作。首先，注意到该语言定义的函数只
能接受一个参数：$&lambda; x. x+1$。然而，这事实上并不会带来任何的限制，为了定义一
个接受两个参数的函数，可以这么写：$&lambda; x. (&lambda; y. x + y)$。也即，为了得
到一个能够接受两个参数的函数，定义一个函数：它接受一个参数，并返回另一个函数：
$&lambda; y. x + y$，这一函数也能接受一个参数。那么，$&lambda; x. &lambda; y. x +
y$便是一个接受两个参数的函数了。

为了简化这一过程，我们可以定义一个语法糖：$&lambda; x. &lambda; y. x + y &equiv;
&lambda; xy. x + y$。

除此以外，为了给这一简单的语言一个不停留于纸面上的实现，我们可以借助Haskell，使
用Haskell的Lambda函数来进行模拟：

```haskell
(\ x y -> x + y) 1 2
-- => 3
```

除此以外，由于不能为表达式或值绑定一个符号（也即定义他们的名称），书写有很大的麻
烦。同一个表达式，出现多少次就需要完整地书写多少遍。为了缓解这一问题，我们可以增
加类似于C macro的预处理过程：$plus := &lambda; xy. x + y; plus 1 2$。注意到这不同
于普通的变量名、函数名定义，虽然形式上非常相似，但这本质上只是与C macro毫无区别
的“替换”。也即，我们无法借助它来达成定义递归函数的目的。举一个直观的例子： $fact
:= &lambda; x. x \* fact (x-1); fact 10$。

本质上，这只会在 _预处理_ 阶段进行 _替换_ ，且只进行一次。
如上面的表达式可以被替换成：$(&lambda; x. x \* fact (x - 1)) 10$。然而计算在这里就
无法继续下去了：现在我们并不知道 `fact` 应该是什么。

对应到Haskell的表示中，我们可以利用Haskell的绑定机制，对一些表达式进行绑定。但为
了让事情保持有趣，我们要信守承诺：不要在任何绑定的表达式中引用自身。

```haskell
fact = \ n -> if n == 0 then 1 else n * fact (n - 1)
-- NO!
```

那么，考虑要解决的问题：在这种情况下，如何实现递归呢？一个很直观的思路是：由于函
数体中事实上只能访问传入的参数，那么，我们可以把函数自己传递给自己：

```haskell
metafact = \ fact n -> if n == 0 then 1 else n * fact (n - 1)
```

那么，如何调用这个函数呢？

```haskell
metafact (???) 3
```

先做一个简单的尝试，我们把 `metafact` 作为参数：

```haskell
metafact (metafact (???)) 3
```

然而，作为参数传入的 `metafact` 也需要有一个能作为“自己”的参数才可以。由于不知道
是什么，我们先用 `undefined` 代替，进行计算：

```haskell
metafact (metafact undefined) 3
=> if 3 == 0 then 1 else 3 * metafact undefined (3 - 1)
=> 3 * metafact undefined 2
=> 3 * if 2 == 0 then 1 else 2 * undefined (2 - 1)
=> 3 * 2 * undefined 1 -- Error!
```

显然，这样是行不通的，但事实上我们已经生成了一部分有用的表达式：$3 &times; 2
&times; &ctdot;$。注意到我们可以得到表达式的前两项，我们可以选取一个小一点的值来计
算：

```haskell
metafact (metafact undefined) 1
=> if 1 == 0 then 1 else 1 * metafact undefined (1 - 1)
=> 1 * metafact undefined 0
=> 1 * if 0 == 0 then 1 else 0 * undefined (0 - 1)
=> 1 * 1 => 1 -- Okay
```

那么，我们也许找到了一条路：我们可以把 `metafact` 嵌套更多层数，这样，就能计算出
更长的表达式：

```haskell
metafact (metafact (metafact (metafact ...))) n
```

事实上，对于嵌套了$n$层的 `metafact` 函数，就能够正确计算最大值为$n-1$的阶乘。例
如，对于嵌套了$1$层的 `metafact` ，仅可计算$n=0$的情况：

```haskell
metafact undefined 0
=> if 0 == 0 then 1 else 0 * undefined (0 - 1)
=> 1 -- Okay
```

那么，自然地，为了得到一个可用的阶乘函数，我们只需要把 `metafact` 嵌套 _无限层_
即可！

无限——多么熟悉。Laziness是与无限打交道的最好方式之一。到了揭晓谜底的时候，考虑下
面的这个函数：$Y = &lambda; f. (&lambda; x. f \\  (x \\  x)) (&lambda; x. f \\  (x \\
x))$。

这个函数有何特殊的呢？进行简单的计算化简：

\\(Y f = (\lambda x. f \ (x \ x)) (\lambda x. f \ (x \ x))\\)

\\(Y f = f \ ((\lambda x. f \ (x \ x)) (\lambda x. f \ (x \ x)))\\)

注意到，这意味着：$Y \\ f = f \\ (Y \\  f)$。

这有什么作用呢？为了看的更清楚：

```haskell
Y metafact
=> metafact (Y metafact)
=> metafact (metafact (Y metafact))
...
=> metafact (metafact ... (Y metafact) ... )
```

于是，我们得到了一个能够嵌套 _无限层_ 的函数，也即我们想要的 `fact` 函数——它能够
正确计算所有正整数的阶乘。

举一个具体的例子：

```haskell
(Y metafact) 3
=> metafact (Y metafact) 3
=> if 3 == 0 then 1 else 3 * (Y metafact) (3 - 1)
=> 3 * (Y metafact 2)
=> 3 * (metafact (Y metafact) 2)
=> 3 * if 2 == 0 then 1 else 2 * (Y metafact) (2 - 1)
=> 3 * 2 * (Y metafact 1)
=> 3 * 2 * metafact (Y metafact) 1
=> 3 * 2 * if 1 == 0 then 1 else 1 * (Y metafact) (1 - 1)
=> 3 * 2 * 1 * (Y metafact 0)
=> 3 * 2 * 1 * if 0 == 0 then 1 else 0 * (Y metafact) (0 - 1)
=> 3 * 2 * 1 * 1 => 6
```

至此，答案已然找到。$Y$这一函数，如同一根神奇的魔术棒，能够让一个任何一个函数进
行 _无限层_ 的嵌套。而归功于Laziness的性质，这种嵌套只会在需要时才会 _展开_ ，因
而不会产生死循环，也不会消耗无穷的内存，而是可以达到我们想要的效果。而利用$Y$函
数的这一特征，我们得以在无法引用自身的情况下，实现函数的递归。

需要注意的是，由于类型系统的限制，将$Y$函数翻译到Haskell中需要小小的作弊：

```haskell
y :: forall a. (a -> a) -> a
y f = f (y f)
```

可以相信，这样定义的 `y` 和之前的$Y$是完全等价的。然而为了绕过类型系统的限制，我
们采用了化简之后的形式，并且不可避免地还是用到了递归，或者说，自引用 _(self
reference)_ 。

借助这一函数，我们可以容易地实现一些递归函数：

```haskell
metamap :: ((a -> b) -> [a] -> [b]) -> (a -> b) -> [a] -> [b]
metamap = \ map f l ->
  match xs with
  | [] => []
  | x :: xs => f x : map f xs
  end
```

> 这里的 `match ... with ... end` 语法并非Haskell中的语法，而是从Coq中借鉴而来。

事实上，$Y$函数常常会被称为 Y Combinator，或是 Fixpoint Combinator。Fixpoint，也
即不动点，为何它会与不动点有关呢？考虑之前定义的 `metafact` 函数：

```haskell
metafact = \ fact n -> if n == 0 then 1 else n * fact (n - 1)
```

有一个显而易见的事实：

```haskell
metafact fact = fact
```

也即，如果将 `metafact` 看作一个函数，这个函数接受一个函数，返回值也是一个函数，
确切来说， `metafact` 的类型为：

```haskell
metafact :: (Int -> Int) -> (Int -> Int)
```

那么，我们想得到的 `fact :: Int -> Int` 函数，事实上为 `metafact` 的 _不动点_ 。

另一方面，注意到 $Y f = f (Y f)$，也即若令 $x = Y f$，则$x = f(x)$。 **$Y f$就是
$f$的不动点！**

所以，我们要得到的递归函数 `fact` 就是 `metafact` 的不动点，而$Y$组合子就可以得
到函数$f$的不动点，所以，$Y$组合子的作用也可以从不动点的角度来解释。这也是$Y$组
合子也被称为不动点组合子的原因。

乍一看，本篇博客的一些假设显得非常怪异，简单地过分，甚至毫无必要。实现递归对于编
程语言而言其实是一件很简单的事情，在汇编代码中，只需要压入不同的参数，再次跳转到
函数的起始地址，就能实现递归调用。

然而，事实上，上文定义的语言近似于无类型的Lambda演算 _(Untyped Lambda Calculus)_
，而它是大多数函数式编程语言的基础。在很多函数式语言的编译器中，代码会被首先编译
为类似于Lambda演算的中间语言 (在Haskell中，这种中间表达的名称为 **Core** ），随后
再编译为更为底层的表示。而很多函数式编程语言中对自引用，或者说递归的实现，背后的
基础也正是$Y$组合子。

至此，对于Laziness的介绍以及对其作用的阐释告一段落了。简单地总结，这两篇博客：

-   介绍了Laziness的含义
-   分析了例子1：Laziness操作无限数列上的作用
-   分析了例子2：Laziness, Y组合子, 递归函数之间的联系
