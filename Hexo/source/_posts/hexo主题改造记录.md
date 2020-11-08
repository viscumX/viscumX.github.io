---
title: hexo主题改造记录
date: 2020-10-26 12:49:53
categories: 记录
tags: [hexo, 个性化]
---

hexo 第三方主题 next 的个性化改造记录

<!--more-->

## 右上角实现 fork me on Github

去[https://tholman.com/github-corners/](https://tholman.com/github-corners/)上按照自己的喜好选择任意一种样式，然后将复制代码到/themes/next/layout/\_layout.swig 中。

具体位置在

```swig
<div class="headband"></div>
```

下方粘贴就好，然后将 href 改为自己的 Github 地址

## 添加头像和社交图标及链接

在/themes/next/\_config.yml 中更改 avatar 下的 url 内容，如果下载到本地的话可以直接更改为图像的地址，这样就能设置好头像了

## next 主题设置 latex 渲染

首先要更换渲染引擎

```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-pandoc --save
```

然后开启 /themes/next/\_config.yml 中的 mathjax ，默认只有在 front-matter 中声明 mathjax：true 时才会对博客中的 latex 公式进行渲染，如果想默认每篇都渲染的话，就把 math 中的 per_page 改为 false 就可以了
