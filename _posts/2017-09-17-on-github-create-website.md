---
layout: post
blog_id: "on-github-create-website"
title: "域名+解析+GitHub来搭建自己的个人网站 "
date: 2017-09-17 00:00:00 -0700
tags: Git
category: Git
summary: 文章主要介绍域名解析，使用GitHub上的开源项目来搭建一个个人博客。并不需要购买服务器，没有数据库访问，适合搭建自己的博客或者个人网站，不适合大型网站
comments: false
---

## 摘要
文章主要介绍域名解析，使用GitHub上的开源项目来搭建一个个人博客。并不需要购买服务器，没有数据库访问，适合搭建自己的博客或者个人网站，不适合大型网站。

##购买域名
* 可以在万网上面注册一个自己的域名，我的是用阿里云账号在万网上面注册的域名。

![](http://upload-images.jianshu.io/upload_images/6673460-15568b6f178089bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 在域名查询栏中搜索自己想要的域名。

![](http://upload-images.jianshu.io/upload_images/6673460-0762d5a676ff6953.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 选择国内的域名提供商还需要通过实名认证，选择实名认证，提交自己的身份信息，一般3-5个工作日。


## 注册GitHub账号，并创建代码仓库
* 可以使用自己的邮箱在GitHub官网注册一个账号，然后就可以开始创建自己的代码仓库了。

![](http://upload-images.jianshu.io/upload_images/6673460-10a75fc0f528a2f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* GitHub为每个注册的用户提供了一个专有的[用户名].github.io（只有一个）这样一个网址，你就可以使用它来和自己购买的域名相关联。之所以不需要购买服务器或者云主机，原因就在GitHub服务器会帮你托管这个[用户名].github.io所用到的全部代码，自动运行。

* 将刚才创建的项目，修改名称为[用户名].github.io，用户名就是你注册GitHub使用的名称。


![](http://upload-images.jianshu.io/upload_images/6673460-fef2c86d10a09e57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 在该项目下新建文件，命名为CNAME，内容为你的域名。 

![](http://upload-images.jianshu.io/upload_images/6673460-d927185d8c314256.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 域名管理控制台>>点击解析，添加信息，记录类型：CNAME，主机记录：可以填自己的顶级域名，记录值一定要是[用户名].github.io，TTL：选择一项。


![](http://upload-images.jianshu.io/upload_images/6673460-8cd9923d8db2d62c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 解析成功之后。网站就建立成功了。

![](http://upload-images.jianshu.io/upload_images/6673460-680d77262a80dd6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 关于网站制作的比较著名开源主题有jekyll、hexo等。之前的文章里面有关于在windows下安装jekyll的，可以进行查看。。。