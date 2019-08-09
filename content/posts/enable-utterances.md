---
title: "在博客中启用Utterances评论系统"
date: 2019-08-09T20:19:22+08:00
showDate: true
tags: ["blog","tutorial"]
---

## What's Utterance?

[Utteranc.es](https://utteranc.es/)是一个基于Github Issues的评论系统。

先前一直苦于为博客找一个合适的评论系统，Disqus很好，但因为众所周知的原因，国内用起来颇多不便。也想过要自己造一个轮子，但迟迟没有开始。

Utterances几乎是无可挑剔的一个选择：基于Github Issues，便捷无阻。长得也看得过去。

##  部署到Hugo中

部署方法很简单，可能也就是几分钟的事情，把官网上的HTML复制到模板中就行啦。

```html
<script src="https://utteranc.es/client.js"
        repo="[ENTER REPO HERE]"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
```

值得一提的是，需要为你选中的Github repo安装Utterances的[App](https://github.com/apps/utterances)才能正常工作。

