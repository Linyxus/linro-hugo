---
title: "让Hugo和reStucturedText和睦相处"
date: 2019-04-04T20:37:33+08:00
draft: false
showDate: true
tags: ["rst", "tutorial"]
---

====
起因
====

| 在写完 `这一篇博客 <https://linyxus.github.io/posts/caculas-1>`__ 之后，我对Markdown彻底失望了，
  尽管它有可爱的Typora和GFM，尽管我已用它写下了无数篇博客。

| 没错，还是数学公式渲染问题。

| Markdown和LaTeX似乎总是两对冤家，因为Markdown滥用的 ``_`` 语法，并且并没有原生的数学公式支持，大部分
  Markdown编译器永远无法完善的解决下标和斜体的恩怨情仇。

| Hugo家的BlackFriday，也不例外。

| 我曾在 `这篇博客 <https://linyxus.github.io/posts/hugo-and-mathjax/>`__ 中自信的以为Hugo官方给出
  的Workaround并无必要，天真的以为Hugo的Markdown编译器不存在这样的问题。现在，我知道我错了。

| 在之前提到的，一篇数学笔记博客中，下面的LaTeX代码在渲染时还是中了 ``<em>`` 的枪。

.. code:: latex
   :number-lines: 1

   $$
     \lim_{k \to +\infty} X_k = A \Leftrightarrow \lim_{k \to +\infty} x_k^{(i)} = a(i), i=1,2,…,n.
   $$

| 所以，我打算转移到更健全的rST。至少rST有专门的Math模块，比如上面出错的LaTeX代码在rST中可以毫无问题的展现：

.. math::

   \lim_{k \to +\infty} X_k = A \Leftrightarrow \lim_{k \to +\infty} x_k^{(i)} = a(i), i=1,2,…,n.

| 和Markdown相比，rST几乎没有什么弱点。Markdown能做到的事情，rST也无所不能。而很多能力，例如完备无冲突的公式
  显示，是过于单薄的Markdown无法企及的。

| 很幸运的，虽然Hugo内置支持的是Markdown， `但是也能通过调用外部程序支持rST
  <https://gohugo.io/content-management/formats/#additional-formats-through-external-helpers>`__ 。
  但毕竟rST不是亲儿子，一开始还是会有些小问题：

======
小问题
======

LaTeX显示
---------

| 因为 ``rst2html`` 对于数学文本块默认是会进行编译的，转成pdf倒还好，但如果转成HTML，显示的效果就非常难看
  （众所周知，HTML对公式的支持非常差）。所以这种任务还是交给MathJax为好，需要修改docutils的配置文件：

.. code:: ini
   :number-lines: 1

   [html4css1 writer]
   math-output: mathjax
                https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js

| 值得一提的是，虽然配置里会需要写上MathJax的URL，但是从最终生成出来的HTML来看，**并没有任何的JS引用。**
  所以，还是自己添加为好，具体办法在官方文档里说的很清楚。

代码高亮
--------

| rST是原生支持代码高亮的（当然Markdown也有），具体语法也和Markdown差不多，给出代码内容和语言类型就可以。
  但是用rST生成的博客文章里的代码块竟然是 **没有颜色** 的。

| 可是没有颜色的代码块又有何用。

| 原因是因为 ``rst2html`` 默认采用了Pygments来高亮代码，而暂时没有在文档里寻觅到可以修改的方法。Hugo原来用
  的也是Pygments，但现在已经换成了Chroma。而且默认采用的是 inline style。可rST使用的Pygments还停留在使用
  classes的阶段。

| 查看生成的HTML发现class都解析正确，只是并没有引入相应的CSS文件。我们需要做的只是把CSS添加到我们的网页中即可。

| 首先用下面的代码配置一下rST中的Pygments，采用短classname。（由于使用的CSS中是short classname）

.. code:: ini
   :number-lines: 1

   [restructuredtext parser]
      syntax_highlight: short

| Hugo的官方文档中也有关于生成 pygments ``style.css`` 的 `叙述
  <https://gohugo.io/content-management/syntax-highlighting/#generate-syntax-highlighter-css>`__ 。
  但是，不要使用，不要使用，不要使用。因为这样生成的CSS文件中会多出一个选择器 ``.chroma`` ，可是rST解析出的
  代码块中并不使用这个类名。

| 还是需要自己用 ``pygments`` 生成一下：

.. code:: bash
   :number-lines: 1

   pygmentize -f html -S monokai > syntax.css

| 然后把生成的 ``syntax.css`` 放入static文件夹，并在模板中引用即可。


======
小缺陷
======

| 有一个缺陷无法弥补，那就是

| ``rst2html`` 实在是太慢了！

| 在使用rST文件之前，网站的编译速度大概是20ms，在使用之后，直接上涨到了200+ms，是之前的十倍之多。所幸花费时间理应
  不是随着博客篇数线性增长的。


========
写在最后
========

rst示例： `Hugo Loves rST <https://linyxus.github.io/posts/hugo-loves-rst/>`__
