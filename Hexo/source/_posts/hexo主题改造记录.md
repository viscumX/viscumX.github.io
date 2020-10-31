---
title: hexo主题改造记录
date: 2020-10-26 12:49:53
categories: 记录
tags: [hexo, 个性化]
---
hexo第三方主题next的个性化改造记录

<!--more-->

## 右上角实现fork me on Github
去[https://tholman.com/github-corners/](https://tholman.com/github-corners/)上按照自己的喜好选择任意一种样式，然后将复制代码到/themes/next/layout/_layout.swig中。

具体位置在
```swig
<div class="headband"></div>
```
下方粘贴就好，然后将href改为自己的Github地址

## 添加头像和社交图标及链接

在/themes/next/_config.yml中更改avatar下的url内容，如果下载到本地的话可以直接更改为图像的地址，这样就能设置好头像了