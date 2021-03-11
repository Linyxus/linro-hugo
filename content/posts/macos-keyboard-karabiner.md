+++
title = "Playing with keyboard on macOS with Karabiner"
author = ["Yichen Xu"]
date = 2019-12-12T17:28:00+08:00
tags = ["tutorial", "macos"]
draft = false
showDate = true
+++

本文会简单地介绍 macOS 上的键盘修改工具 Karabiner，并且简单介绍我配置的修改方式。


## 引子 {#引子}


### 关于 macOS 的输入法 {#关于-macos-的输入法}

在macOS上，一直让我很头疼的一件事情是自带的输入法对中文输入不是非常友好。由于对
中文输入者而言，在中英文之间切换是相对常见的操作，大多数人习惯使用Shift切换，因
为绝大多数中文输入法都默认这一点。但macOS上默认使用Capslock。

这并不是关键之处，让我完全不能好好使用macOS自带输入法的根本原因会在下面提到：我
需要把Capslock用作他用。

事实上，第三方输入法我也尝试过，从国产经典的搜狗，到较为小众的落格。搜狗的毛病在
于有广告，界面也未免太花里胡哨，而落格的致命之处在于词库实在太难用，有时候首选词
叫人完全无法理解，不仅如此，落格最主要的卖点：双拼，也对使用全拼的我毫无吸引力。

通过 Karabiner，可以把Shift配置为我想要的样子。


### 关于修改CapsLock {#关于修改capslock}

修改CapsLock应该是很多程序员的必备操作了。Capslock的位置处于整张键盘上最容易按到
的地方之一，却基本是最少使用到的按键，这很不合理。

我们可以把Capslock转而映射到Control（对于Emacs用户）或Escape（对于Vim用户）来很
大地提高键盘的使用效率。

之前发现了一个很棒的工具，名为[Xcape](https://github.com/alols/xcape)，能够 **同时** 把Capslock映射到Control与Escape，
这很容易实现——Escape和Control的使用事实上是不重合的，使用场景有明显的差异：
Control是修饰键，与其他按键一起按下，而Escape常常是独立按下的。也可以通过按下的
时间来区分他们。

很遗憾的是，Xcape **并不支持macOS** 。

但 Karabiner 弥补了这一遗憾。


## Karabiner的安装 {#karabiner的安装}

[Karabiner](https://pqrs.org/osx/karabiner/)

Karabiner 的安装非常简单，直接在官网上下载dmg，双击安装即可。

值得一提的是，Karabiner的官网上也有一个官方整理的 Complex modifications [集合](https://pqrs.org/osx/karabiner/complex%5Fmodifications/)，大
多数常用的修改方式只需要 import 这上面的修改即可，修改的定义方式是json，语法也颇
为直观，必要时可以修改。


## Modification I: 使用Shift切换中英文 {#modification-i-使用shift切换中英文}

利用 Shift 切换中英文有一个很直观的实现：单独按下时，发送Capslock，否则仍然发送
Shift。这个实现在上面提到的集合中就有 [现成的](https://pqrs.org/osx/karabiner/complex%5Fmodifications/#shift)。

然而，在使用过程中，我发现了一些问题：在一些情况下，如Chrome和PyCharm，这样的实
现会产生一些非常奇怪的Bug：输入的英文皆为大写。尝试解决无果，我转向了第二种方案：
类似实现，只不过直接发送切换输入法的快捷键，而不是Capslock。

```javascript
{
    "description": "Change left_shift to control+option+space if pressed alone (rev 2)",
    "manipulators": [
        {
            "from": {
                "key_code": "left_shift",
                "modifiers": {
                    "optional": [
                        "any"
                    ]
                }
            },
            "to": [
                {
                    "key_code": "left_shift"
                }
            ],
            "to_if_alone": [
                {
                    "hold_down_milliseconds": 100,
                    "key_code": "spacebar",
                    "modifiers": [
                        "left_control",
                        "left_option"
                    ]
                }
            ],
            "type": "basic"
        }
    ]
}
```

我切换输入法的快捷键是 control + option + space，这样的实现到目前为止完全符合预
期，毫无问题。


## Modification II: Capslock {#modification-ii-capslock}

Capslock的修改同样有[现成的](https://pqrs.org/osx/karabiner/complex%5Fmodifications/#caps%5Flock)。

Capslock这样的修改方式对Evil用户尤其友好——既兼顾了Evil的Escaping，又保护了Emacs
用户的小指。

这样修改过后，在Emacs中编辑基本不再需要离开键盘的中间区域了。


## Modification III: System-wide Vi-style navigation {#modification-iii-system-wide-vi-style-navigation}

第三个修改是针对方向键的，也即在全系统层面，用一个修饰键加上hjkl来作为方向键。

这一修改同样有 [现成的](https://pqrs.org/osx/karabiner/complex%5Fmodifications/#vi%5Fstyle%5Farrows) 。

为了最大化的避免与Emacs中的快捷键发生冲突，我选用了option键。虽然按起来没有
Capslock来的方便，但至少也比方向键要快得多。
