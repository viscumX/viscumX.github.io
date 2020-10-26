---
title: hexo多端同步
date: 2020-10-26 11:09:23
categories: 配置环境
tags: [hexo, 同步]
---

之前只在实验室的电脑上配置了一下hexo的相关功能，回到宿舍之后想再玩一玩就没有办法了。于是在个人笔记本和实验室电脑上同步了一下hexo。具体操作如下

<!--more-->

## 实验室电脑上传hexo至Github
### 同步hexo源文件
在之前的repository下新建一个hexo分支，并且到settings中修改hexo为default（默认）分支
```bash
git checkout -b hexo # 创建新分支并切换到该分支
git push origin hexo # 同步当前分支
```
同步hexo源文件夹到Github上
```bash
git add .
git commit -m "xxx" # xxx随便写什么
git push origin hexo 
```
### 同步第三方主题
因为在实验室电脑上配置的主题next是直接从别人的工程下git下来的，所以无法直接同步到Github上，到repository上查看发现/themes/hexo-theme-next也无法打开。因此要对第三方主题也进行同步，这里直接用next主题举例

首先~~忍痛~~删除之前的第三方主题文件夹，然后去官方的next项目下fork一个到自己的仓库中，然后git自己fork下来的项目
```bash
# 将第三方主题添加到hexo分支的子模块上
git submodule add https://github.com/yourname/hexo-theme-next themes/next 
```
再同步一下hexo文件

## 个人电脑同步hexo博客
### 同步hexo源文件
新电脑上需要下载node和git，这里就不赘述了，可以查看之前的一篇博客

安装hexo及其依赖，并且下载自己的博客项目
```bash
npm install hexo-cli -g # 安装hexo
git clone -b hexo git@github.com:yourname/yourname.github.io.git
npm install # 安装相关依赖
```
### 同步第三方主题
在博客根目录中执行以下命令，初始化子模块并更新
```bash
git submodule init
git submodule update
```
如果对第三方主题有什么修改的话，进入/theme/next目录
```bash
git add .
git commit -m "xxx" # xxx随便写什么
git push origin master 
```
假如有什么博客更新的话
```bash
git add .
git commit -m "xxx" # xxx随便写什么
git push origin hexo 
```