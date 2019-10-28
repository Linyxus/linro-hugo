---
title: "简短的gdb使用手册"
date: 2019-10-24T12:00:42+08:00
showDate: true
draft: false
tags: ["notes"]
---

## 编译

为了使代码能够被gdb调试，需要在编译时加入`-g`选项，也即

```shell
gcc -g main.c -o main
```

使用`-g`选项后，gcc会为生成的程序加载调试符号，供给gdb载入。

对编译完成的文件，使用gdb载入：

```shell
gdb main
```

## 基本执行控制指令

### `run`，`break`，`print`

三个指令的使用示例：
