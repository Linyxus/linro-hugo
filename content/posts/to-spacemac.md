+++
title = "Switching From Purcell's Emacs.d to Spacemacs"
author = ["Yichen Xu"]
date = 2019-11-07T10:32:00+08:00
tags = ["tutorial", "emacs"]
draft = false
showDate = true
+++

很久以来，我的Emacs配置都基于无比经典的Purcell的 [emacs.d](https://github.com/purcell/emacs.d)。但就在最近，我突然决定切换到 Spacemacs。

我曾用过一段时间的 Spacemacs，或者，确切的说，贯穿了我高二到高三的大部分时光。但
彼时对Emacs了解颇少，对Spacemacs的使用也仅限于 uncomment 几个 layer，或是复制粘
贴几句配置到 user-config 中。大一之后，一个偶然的契机，我打算更深入地学习Emacs。
那时候开始，我一直使用并轻度定制了Purcell的Emacs配置。
[Github repo](https://github.com/Linyxus/emacs.d/commits/master)

但老实说，不管是配置的方法还是版本管理，我在这段时间里做的真的有些随便。


## 切换到 Spacemacs 的理由 {#切换到-spacemacs-的理由}

1.  Evil! Spacemacs是在emacs中使用evil的最快速的方法之一
2.  系统化的配置，基于 use-package，Purcell的配置方法过于古老不便
3.  Battery included，庞大的社群： [Github](https://github.com/syl20bnr/spacemacs) 上已有18.8k个星星
4.  丰富的文档，相较于 Purcell 的配置基本无可用文档


## Migration {#migration}

事实上，除了一些无关紧要的配置，我几乎可以无痛迁移到 Spacemacs，因为 Spacemacs 的 layer 实在是太齐全丰富了。

日常使用中，我会使用Emacs编辑的文件类型其实不是很多：

-   C/C++
-   Org
-   Haskell
-   \\( \LaTeX{} \\)

平时虽然Python写得也比较多，但是，PyCharm真香！

上述我要用到的编辑环境，所有在 Spacemacs 中都有现成的 layer 可以用，我要做的只是
在 `dotspacemacs-configuration-layers` 中把他们加上罢了。


## 迁移过程 {#迁移过程}

首先，删除原来的emacs.d。

```bash
rm .emacs.d // my .emacs.d is just a symlink
```

然后

```bash
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
```

打开Emacs。Voila！


## 一些常用Layer与基本配置 {#一些常用layer与基本配置}

-   auto-completion，编辑器必备，看着舒服
-   helm，也可以选ivy，个人觉得的确是ivy简洁好看一些，但helm是默认之选，官方文档中
    也提到如果选择ivy，一些功能也许不可用。[Documentation](https://github.com/syl20bnr/spacemacs/blob/master/doc/DOCUMENTATION.org#completion)
-   org，不得不说，Spacemacs自带的org layer实在是太好用了！

除此以外，因为看惯了Purcell用的Tomorrow系列theme，对spacemacs自带的theme系列有些
不习惯。而且spacemacs-dark在org下实在是太难看了些，故而还是配了一下tomorrow系列
的theme，看起来很习惯。
![](/ox-hugo/spacemacs.png)

只需要把tomorrow theme对应的package加到dotfile的additional-packages中即可。

```emacs-lisp
dotspacemacs-additional-packages '(color-theme-sanityinc-tomorrow)
```

别忘了选一个color theme作为默认。

```emacs-lisp
dotspacemacs-themes '(sanityinc-tomorrow-eighties
                      spacemacs-dark
                      spacemacs-light)
```


## Version Control {#version-control}

Spacemacs的版本控制有很多种方式。简单来说，Spacemacs的配置文件分为以下部分

1.  Spacemacs主体，也就是 `.emacs.d` 中的大部分内容。这一部分至少对我而言并不需要
    进行版本控制，因为我基本不会对其进行修改，直接跟着上游就可以了。
2.  .spacemacs。也就是我基于Spacemacs的配置文件。
3.  Private layers。也就是自己写的layer。

文档中有所提及的是，private layer所在的文件夹是会被Spacemacs主git repo忽略的。如
果要对我们的自用layer进行版本控制，可以简单地在private文件夹建立一个repo，或是把
一个repo symlink到private文件夹，效果类似。

但这么做有一个巨大的弊端：.spacemacs和private layer不能很好地放在一起管理。有一
个显然的解决方案：建一个管理Spacemacs配置文件的repo，里面包含一个.spacemacs文件，
一个private文件夹，再分别symlink到相应位置，但这样非常不自然。

我最终选择了另一个解决方案。[文档](https://github.com/syl20bnr/spacemacs/blob/master/doc/QUICK%5FSTART.org#dotdirectory-spacemacsd)中提及，除了用一个.spacemacs文件作为配置以外，
Spacemacs也可以像Emacs一样，把一个文件夹作为配置来源。具体地，新建一个文件夹
`~/.spacemacs.d` ， `~/.spacemacs.d/init.el` 即可作为原来的 `.spacemacs` 。

借助于这一特性，可以把我的Spacemacs配置和private layers集合到一个文件夹中：

```nil
.spacemacs.d
├── LICENSE
├── README.org
├── init.el
└── layers
    └── ox-hugo-layer

2 directories, 3 files
```
