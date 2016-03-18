---
layout: post
title: "Githup＋Octopress搭建静态博客"
date: 2014-11-08 21:14:52 +0800
comments: true
categories: intro
---


## Overview
**octopress**,一个非常简易方便的**静态**博客框架。优点是可以利用Github的Github Pages快速搭建， 并且简洁方便，支持**markdown**语言,对经常需要插入代码的技术博客来说十分易用；缺点是对搜索、分类等复杂功能支持的不好，也不支持评论。
### steps
1. 安装ruby，推荐1.9版本
2. 安装Git
3. 下载octopress
4. 部署到github上


### install ruby & git
不同的系统环境安装步骤不一，我的是**MacOSX10.10**。
#### 1.自带ruby
```bash
ruby --version
# 显示为 2.0
```
更改gem的源为国内源，并删除原来的源，这样下载模块时会不卡
```bash
gem sources -a http://ruby.taobao.org/
gem sources -r https://rubygems.org/
gem sources -l
```
最后一个命令是查看当前的源列表。
#### 2.安装python的pygments模块（不知道有什么用）
```bash
sudo easy_install pygments
```

### 下载octopress

**octopress**的github地址<https://github.com/imathis/octopress.git>
```bash
git clone git://github.com/imathis/octopress.git  octopress
```
将其下载到当前目录的octopress目录下,然后更新octopress 的gem 源
将`Gemfile`文件里的
```
source = "http://rubygems.org/"
```
改为
```
source = "http://ruby.taobao.org/"
```
最后安装octopress的依赖项,这些操作都是在**octopress**目录下进行
```bash
sudo gem install bundler
bundle install
```

### 新建Gihub Repositories
新建一个repo, 命名为你的`username.github.io`，这样就可以自动使用github pages的域名访问，详细可以参考<https://help.github.com/categories/github-pages-basics/>

#### 将octopress目录与你的github repo 链接
获得你的`repo地址`, 在下面命令的提示中输入。
```bash
rake setup_github_pages
```

#### 安装
```bash
rake install
rake generate
rake preview
```
三个命令分别是安装主题，产生静态文件，预览；而我的预览功能是出错了，不能在本地的`localhost:4000`地址访问

###  部署
```bash
rake deploy
```
你需要理解这个过程。
> 1. source目录里放置着你的md文件格式的博客， 在posts中
> 2. public则是最后展示的静态博客系统
> 3. .themes里面放着主题皮肤，每个主题皮肤里不外乎是source目录和sass目录，可以理解为css文件等

deploy命令首先会从source何sass目录中，产生博客的静态文件，位于public目录，然后清空_deploy目录，拷贝public 目录。
_deploy目录对应你的`github repo`的master分支，也是你博客的所有静态文件。
整个目录中包含两个仓库，origin仓库既是你的_deploy目录下的静态工程;octopress仓库则是整个octopress工程;有些教程会教你使用origin 的source远程分支来备份source 里面的文件。
如果你的git repo 本来就有文件的话,你需要进入_deploy 目录，删除所有文件，不然deploy命令会出错。

### 更换主题

可以从网上下载很多octopress的主题，在官网里也有推荐列表<https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes>

安装命令如下
```bash
cd octopress
git submodule add GIT_URL .themes/THEME_NAME
rake install['THEME_NAME']
rake generate
```

### 博客配置

更改标题等，修改_config.yml文件
```
url: http://neilsoul.github.io
title: My Octopress Blog
subtitle: A blogging framework for hackers.
author: Your Name
simple_search: https://www.google.com/search
description:
```

### 写博文

```bash
rake new_post['blog name']
```
则会在source 的posts目录中看到相应的文件

### 部署更新
```bash
rake generate
rake deploy
```