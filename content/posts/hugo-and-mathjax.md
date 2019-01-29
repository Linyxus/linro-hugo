---
title: "Hugo & Mathjax"
date: 2019-01-29T22:07:57+08:00
showDate: true
tags: ["text"]
---

Mathjax是什么就不多说了，$\LaTeX$公式网页渲染最常用的lib。

<!--more-->

## 配置

Hugo的官方[文档](https://gohugo.io/content-management/formats/#mathjax-with-hugo)中已经很贴心的写明白了如何引入&&配置Mathjax。

但文档里有一些很奇怪的地方。里面提到了一个Markdown和Mathjax一起用存在的issue：

> After enabling MathJax, any math entered between proper markers (see the [MathJax documentation](https://docs.mathjax.org/en/latest/)) will be processed and typeset in the web page. One issue that comes up, however, with Markdown is that the underscore character (`_`) is interpreted by Markdown as a way to wrap text in `emph` blocks while LaTeX (MathJax) interprets the underscore as a way to create a subscript. This “double speak” of the underscore can result in some unexpected and unwanted behavior.

大概就是，$\LaTeX$中表示下标会引入`_`，但很不巧的是`_`会被Markdown处理为斜体的标记，导致异常。于是文档里给了一些solution，前两个都不靠谱，或者很麻烦。

一个是给每个`_`都加上转义`\_`，这样Markdown Parser就不会动这个`_`了，但真的很麻烦，而且在Typora中的预览看起来也很难受。

另一个是把Math Block包在`div`里，但inline math还是无法解决。

最后一个solution更加麻烦，利用\`\`之间的字符，也就是inline code，不会被处理来解决。但是inline code的字体不一样啊，所以还得引入css和js判断来work around。

这个issue的确是存在的，以前用Hexo的时候，就日日苦于这个问题——还找不到方法解决。本来想写一个脚本把文章里的Math Block批量转成服务器渲染的外链图片，一劳永逸，但一直没动手。一直以来用的是`\_`的work-around。但Hugo中，这个issue似乎是不存在的。

Hugo的Markdown后端用的是Blackfriday，在[文档](https://gohugo.io/content-management/formats/#blackfriday-extensions)里关于Blackfriday的部分可以看到，Blackfriday其实有这个extension：



>*`noIntraEmphasis`*
>
>default: *enabled*
>Purpose: The “_” character is commonly used inside words when discussing code, so having Markdown interpret it as an emphasis command is usually the wrong thing. When enabled, Blackfriday lets you treat all emphasis markers as normal characters when they occur inside a word.

而且默认就是打开状态。

所以按理来说，就算是Mathblock里面一般下划线的使用，比如：`a_1 = b_1`。也都会被识别为in-word emphasis，不作处理。
$$
a_1 = b_1
$$
嗯，没有问题。

但其实一直想吐槽的是，这个issue，无论如何，明明有一个更简单的解决办法：不用`_`，而用`*`，并且在parser中直接disable对`_`的识别。然而，这个solution似乎不怎么受欢迎？也许是使用习惯问题。

## 测试

$$
x_{1, 2} = \frac {-b \pm \sqrt{b^2-4ac}} {2a}
$$

$$
e^{i\pi} + 1 = 0
$$

$$
a^{p-1} \equiv 1 \pmod p
$$

$$
f,g \in C[a, b] \cap D(a, b)
\Rightarrow \forall x \in (a, b), g'(x) \neq 0, \exists \xi (a < \xi < b), \\
\frac{f(b)-f(a)}{g(b)-g(a)} = \frac{f'(\xi)}{g'(\xi)}
$$

