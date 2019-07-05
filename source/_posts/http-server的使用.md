---
title: http-server的使用
date: 2017-02-28 10:27:52
tags: http-server
---
# http-server


利用cmd命令窗口快速建立一个静态服务器

<!--more--><!--more-->


## 全局安装

    npm install http-server -g 

## 将服务器的根目录设置为某个项目目录

1. 进入某个目录结构
2. shift+右键  从此处打开命令窗口
3. 在命令窗口输入`http-server`或者`hs`
4. 用浏览器访问`http://localhost:8080/`即可

## 端口号被占用
    hs -p  设置端口号

    hs -o  直接用浏览器访问

## 使用

  这样就可以离线查看下载好的框架内部的doc中的网页了，在docs的目录下

      hs -o -p不常用的端口号即可