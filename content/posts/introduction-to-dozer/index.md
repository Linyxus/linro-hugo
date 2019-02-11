---
title: "Dozer: 用两个小点管理你的macOS状态栏"
date: 2019-02-11T13:20:02+08:00
showDate: true
tags: ["macOS"]
---

macOS的状态栏设计是个饱受诟病的地方。因为从系统层面上，并不存在管理状态栏的功能（除了排序）。

{{<figure src="statusbar.png">}}

由于没有隐藏图标的功能，所以很多从来用不着也不想看的图标总是占着位置。比如Spotlight的放大镜，比如Siri的圆圈。

对于这个问题有不少流行的解决方案，Bartender是主流，也有Vanilla之流紧随其后。

{{<figure src="apps.png">}}

只是光是Bartender丑丑的icon就让人望而却步，Vanilla虽然icon尚算得上好看，但是它隐藏状态栏图标的实现非常丑陋，似乎是用一个窗口遮盖，这种连hack都是抬举的实现办法带来三个问题：

1. 由于不同应用程序的菜单栏长度不同，切换应用程序后遮盖菜单栏是家常便饭
2. 切换到Mission Control时用于遮盖的窗口会原形毕露
3. 变色壁纸变色时遮盖窗口来不及变色

而最重要的是，这两个家伙都要钱，价钱算不上贵，但由于他们一个长得丑，一个缺胳膊少腿，为了这种货色花钱心里总不那么舒服。

当然，也可以去一些网站上找hack过的免费解锁版本，这也是种选择。

但前些日子，我在万能的知乎上找到了几乎无可挑剔的另一个选择：

## Dozer

Dozer是完全开源的，Github主页在[此](https://github.com/Mortennn/Dozer)。

具体的安装&使用方法在Github上都有，这里小小一提：

### Install

```
brew cask install dozer
```

### Usage

*略*

Github上的演示：

![image-20190211134321152](https://github.com/Mortennn/Dozer/raw/master/demo/demo.gif)