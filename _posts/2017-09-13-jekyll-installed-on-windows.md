---
layout: post
blog_id: "jekyll-installed-on-windows"
title: "Windows上安装Jekyll "
date: 2017-09-13 00:00:00 -0700
tags: Jekyll
category: Jekyll
summary: jekyll是一个简单的免费的Blog生成工具,是一个静态站点生成器,它会根据网页源码生成静态文件。它提供了模板、变量、插件等功能,所以实际上可以用来编写整个网站。本篇将介绍如何在Windows下安装jekyll。
comments: false
---

#介绍Jekyll
* jekyll是一个简单的免费的Blog生成工具,是一个静态站点生成器,它会根据网页源码生成静态文件。它提供了模板、变量、插件等功能,所以实际上可以用来编写整个网站。本篇将介绍如何在Windows下安装jekyll。

#安装Ruby
* [Ruby官网下载地址](https://rubyinstaller.org/downloads/)   本次使用的版本是：rubyinstaller-2.3.3-x64.exe

![](http://upload-images.jianshu.io/upload_images/6673460-adb62269c7b857b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 勾选”Add Ruby executables to your PATH” 进行安装,完成后打开命令行工具检测Ruby是否安装成功。输入"ruby -v"后能够看到如下界面。

![](http://upload-images.jianshu.io/upload_images/6673460-35160740686a3f80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#安装DevKit
* 下载地址跟Ruby下载地址相同，选择适合操作系统的版本，我的window7是64位操作系统的，所以选择的版本是DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe


![image.png](http://upload-images.jianshu.io/upload_images/6673460-0e78a582f56a3f02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


* 安装在D:\DevKit下，接下来，需要初始化devkit并将其绑定到Ruby安装。


![](http://upload-images.jianshu.io/upload_images/6673460-f25da28d2d68c397.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 如果出现如下报错的：

![](http://upload-images.jianshu.io/upload_images/6673460-4fdf082b1af7a303.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 * 排查后得知，DevKit不能只配置一项，就算只有一项，也要配置二个相同的项，才能安装成功。于是，修改配置如下：

![](http://upload-images.jianshu.io/upload_images/6673460-b0ca471116c59852.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#安装Jekyll
* 打开命令行输入以下命令 " gem install jekyll "，如果顺利会出现如下情况,则表示正在安装,可能需要一段时间,需要下载的东西较多,也取决于你的网速。

![](http://upload-images.jianshu.io/upload_images/6673460-b6b02e64aab708dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 检查jekyll是否安装成功

![](http://upload-images.jianshu.io/upload_images/6673460-761d5a1da285a9de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 安装Python
* [Python官网下载地址](https://www.python.org/downloads/) ，本次使用的版本是：python-3.6.2.exe，当出现如下对话框 选中：”Add Python 3.6 to PATH”

![](http://upload-images.jianshu.io/upload_images/6673460-035b1945acc455a7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#安装pip
* pip是一个Python包的安装和管理工具。会需要它的安装pygments,pygments.rb突出代码。
 [pip下载地址](https://pypi.python.org/pypi/pip)，下载后解压，然后用命令行工具进入到pip的解压目录，

![](http://upload-images.jianshu.io/upload_images/6673460-ceaf0b3cca4ed03e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 完成之后在F:\python\Scripts 的Scripts文件夹中可以看到pip.exe 运行程序，说明pip安装成功。接下来安装pygments。

![](http://upload-images.jianshu.io/upload_images/6673460-3332509077b43b0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 设置Pygments作为语法高亮，在F:\DevKit\config.yml中添加如下：highlighter: pygments

#启动Jekyll
```
jekyll new myblog  
cd myblog  
jekyll serve 
```

![](http://upload-images.jianshu.io/upload_images/6673460-ab3751bde811e027.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](http://upload-images.jianshu.io/upload_images/6673460-8602d6b32f4cbf56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*  服务启动成功，在浏览器中输入：http://localhost:4000。


![image.png](http://upload-images.jianshu.io/upload_images/6673460-3cdf06821021f5be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 在服务启动的过程中，可能会报各种错误；
1.这个是由于没有安装bundler，需要安装，在命令行中输入" gem install jekyll bundler "。
![](http://upload-images.jianshu.io/upload_images/6673460-a8045092694ea376.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/6673460-5d3f725220ab18f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.这个是由于没有安装minima，在命令行中输入" gem install minima "进行安装。

![](http://upload-images.jianshu.io/upload_images/6673460-87d2fe57d4c8955b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.这个是由于没有安装tzinfo-data，在命令行输入" gem install tzinfo-data "进行安装。

![](http://upload-images.jianshu.io/upload_images/6673460-777abca69728b6b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 在启动服务的过程中可能会报各种各样的错误，更多是" Count not ***  in any of the gem sources listed in your .... "，找到里面缺少的***，再进行安装，重启服务。


文章参考来源：[麦田技术博客](http://blog.itmyhome.com/2015/01/jekyll-installed-on-windows)