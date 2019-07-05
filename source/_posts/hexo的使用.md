---
title: hexo的使用
date: 2017-03-01 13:38:17
tags:
---

# 前言

hexo是一款基于Node.js的静态博客框架。



<!--more--><!--more-->

# 配置环境 



1. Node

	作用：用来生成静态页面的


2. Git

	作用：管理本地的hexo内容并且同步到github上


3. 申请GitHub（必须）

	作用：是可以利用github-page搭建自己的博客





# hexo的安装

首先创建一个文件夹专门来存放的blog的内容

	npm install -g hexo   全局安装

	hexo init      生成需要的文件

更变主题

修改当前目录下的_config.yml中的 themes文件和主题文件名相同
头像的头文件目录是source文件


# github部分 

## 创建repository

这里需要注意的是：Repository name格式必须为youname.github.io

## 部署本地文件到github

修改_config.yml文件的内容

	deploy: 
	  type: git
	  repository: http://github.com/yourname/yourname.github.io.git
	  branch: master








# 新建一篇文章


	hexo new post "新建文章" #简写形式 hexo n "新建文章"
	打开新建的md文件：
	---
	title: my new post #可以改成中文的，如“新文章”
	date: 2015-04-08 22:56:29 #发表日期，一般不改动
	categories: blog #文章文类
	tags: [博客，文章] #文章标签，多于一项时用这种格式，只有一项时使用tags: blog
	---
	#这里是正文，用markdown写，你可以选择写一段显示在首页的简介后，加上
	<!--more-->#在<!--more-->之前的内容会显示在首页，之后的内容会被隐藏		
	，当游客点击Read more才能看到。


	hexo clean #清除旧的public文件夹
	hexo generate #生成静态文件 简写形式 hexo g
	hexo deploy #发布到github上 简写形式 hexo d


# 免密码发布


## 生成 SSH 密钥

	$ cd ~/.ssh
	$ ssh-keygen -t rsa -C "your_email@example.com"
	三个回车
	
- 找到id_rsa.pub文件
- 在github里的setting中添加ssh
- 验证配置是否成功

		$ ssh -T git@github.com
		Hi username! You've successfully authenticated, but GitHub does not
		provide shell access.