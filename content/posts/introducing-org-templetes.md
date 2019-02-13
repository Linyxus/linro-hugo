---
title: "org-mode规划笔记"
date: 2019-02-13T12:05:39+08:00
showDate: true
tags: ["emacs", "note"]
---

org-mode可能是Emacs最有名的一部分。作为一个瑞士军刀般的笔记系统，org-mode真的可以渗入你生活的每个角落。

之前我也在学校社团里做过一个有关org-mode的讲座。关于org-mode，记忆深刻的是某知乎用户的一句话：

> 我的生活就是在org-mode掌控之下的好吗

那么，直接给出我使用org-mode的一些规划和设计吧。

## .org

### gtd.org

在这个文件里放入日常事务，要完成的事情。

比如：打印四级准考证、整理行李、去开会、做推送等等。

### idea.org

冒出来的想法，但不一定要实现。如果要实现，归档到gtd.org中完成。

比如：写一篇博客介绍我的org-mode使用方法、用Clojure写一个小游戏等等。

### learning.org

学习规划。

比如学习Python，学习Haskell，学习各种算法等等。

### book.org

书籍归档。

### project-specific org file

各个project专用的.org文件，放在project根目录内。

## Capture templates

### journal

小总结，小感想。

总结今天做了什么。（对抗焦虑症）

### todo

要做的事情。

### idea

突然冒出来的想法。

### Source code

列一下org template配置的elisp代码：

```lisp
(setq org-capture-templates
      '(("j" "journal" entry (file+datetree "~/org/journal.org")
         "* %?\n%U\n")
        ("t" "todo" entry (file+headline "~/org/gtd.org" "Inbox")
         "* TODO %?\n%U\n")
        ("i" "idea" entry (file+headline "~/org/idea.org" "Inbox")
         "* TODO %?\n")))
```



