---
title: 使用hexo和GitHub搭建个人博客
date: 2020-10-23 10:16:48
categories: [配置环境]
tags: [hexo]
---

之前一直是用csdn来写博客，但是也觉得csdn的界面不够简洁美观。最近突然发现了一个很炫酷的个人博客，稍微探索了一下之后，决定自己也整一个

下面详细介绍一下流程

<!--more-->

## Node.js的安装

这一步相当简单，去[官网](https://nodejs.org/zh-cn/)上下载之后无脑安装就行了

## git的安装

也是一样无脑安装[https://git-scm.com/](https://git-scm.com/)


## hexo的安装

重头戏终于来了

首先在自己合适的位置新建一个文件夹，命名随意，进入文件夹，右键运行```Git Bash Here```，在命令行里运行

```bash
npm install hexo-cli -g  #安装hexo
```

然后在这个文件夹下再新建一个名为hexo的文件夹，进入文件夹，右键运行```Git Bash Here```，运行

```bash
hexo init  # hexo初始化
npm install hexo-deployer-git --save  # 安装deploy的相关文件
```

接下来检查一下效果

```bash
hexo g  # 生成
hexo s  # 启动本地服务器
```
显示 ```INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.``` ，打开[http://localhost:4000](http://localhost:4000)，出现网页效果则说明成功。

## 关联博客与Github

在GitHub上新建一个repostitory，以 "yourname.github.io" 命名。在最外层的文件夹，即第一个新建的文件夹内部右键运行```Git Bash Here``` ，输入

```bash
ssh-keygen -t rsa -C "your email@example.com"  # 自己的邮箱
```

接着直接三个回车，就会出现生成的密钥，运行

```bash
clip < ~/.ssh/id_rsa.pub  # 复制密钥到剪切板
```

进入GitHub账户的settings中的SSH and GPG Keys，选择 ```New SSH key``` 粘贴刚刚的密钥到 ```key``` 中，```title``` 自己定一下就好。

在 git bash 命令行中运行

```bash
ssh -T git@github.com 
```
会提示 ```Are you sure you want to continue connecting (yes/no)?``` ，选择 yes 显示successfully authenticated则说明成功。

再设置一下个人信息

```bash
git config --global user.name "yourname"  
git config --global user.email  "yourname@gmail.com"  # 自己的邮箱
```

打开Github上的项目，在 ```clone or download ``` 中选择use SSH，复制地址到剪切板。打开/hexo/_config.yml文件，更改最后的deploy为以下形式

```YAML
deploy:
  type: git
  repository: # 此处填上刚刚复制的内容
  branch: master
```
之后也可以在这个文件中修改博客的样式。

执行

```bash
hexo clean # 清除缓存文件和已生成的静态文件
hexo deploy -g # 更新GitHub上的部署
```
访问 https://yourname.github.io 就可以访问个人博客了
