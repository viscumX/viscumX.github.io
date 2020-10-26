---
title: hexo主题改造记录
date: 2020-10-26 12:49:53
categories: 记录
tags: [hexo, 个性化]
---
hexo第三方主题next的个性化改造记录

<!--more-->

## 右上角实现fork me on Github
去[https://tholman.com/github-corners/](https://tholman.com/github-corners/)上选择一种样式，复制代码到/themes/next/layout/_layout.swig中。

具体位置在
```swig
<div class="headband"></div>
```
下面粘贴就好，然后将href改为自己的Github地址 