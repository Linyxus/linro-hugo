---
title: "利用cron自动同步org到Github"
date: 2019-02-26T22:58:41+08:00
showDate: true
tags: ["note", "org"]
---

## 效果

{{<figure src="p1.png">}}

每天在固定的时间，只要电脑开着，自动commit发生在org文件夹内的改动，并且push到Github上。

如图，设定的是在所有的整点都会自动更新。

## 步骤

### #1 编写自动同步shell

```shell
#!/bin/bash

echo -e "\033[0;32mPulling from GitHub...\033[0m"

git pull

git add .

git commit -m "Auto update : `date`"

echo -e "\033[0;32mPushing to GitHub...\033[0m"

git push origin master
```

在`echo`的文本里奇奇怪怪的内容都是`escape character`s，目的是更更改文本的颜色（为绿色）。目的是看起来更加舒服。（可是自动运行的时候你根本看不见啊喂）

### #2 增加cron task

在命令行中输入：

```shell
$ EDITOR=nano cron -e
```

增加这一行：

```cron
0 * * * * cd ~/org && ./update.sh
```

保存退出即可。

> *注意！注意！注意！*
>
> macOS下由于一些奇怪的权限原因，只能使用Terminal.app进行编辑。
>
> iTerm2是不行的。

### #3 Enjoy it!