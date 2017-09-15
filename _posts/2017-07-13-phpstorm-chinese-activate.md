---
layout: post
blog_id: "phpstorm-chinese-activate"
title: "PhpStorm2017版激活方法、汉化方法以及界面配置 "
date: 2017-07-13 00:00:00 -0700
tags: PHP
category: PHP
summary: PhpStorm是一个轻量级且便捷的PHP IDE，其旨在提高用户效率，可深刻理解用户的编码，提供智能代码补全，快速导航以及即时错误检查
comments: false
---

[PhpStorm激活和汉化文件下载网址](http://pan.baidu.com/s/1nuHF1St)（提取密码：62cg）

**PHPMailer的介绍**

PhpStorm是一个轻量级且便捷的PHP IDE，其旨在提高用户效率，可深刻理解用户的编码，提供智能代码补全，快速导航以及即时错误检查。

[PhpStorm安装包下载网址](http://www.jetbrains.com/phpstorm/)

**优点**

1、跨平台
2、对PHP支持refactor功能。
3、自动生成phpdoc的注释，非常方便进行大型编程。
4、内置支持Zencode。
5、生成类的继承关系图，如果有一个类，多次继承之后，可以通过这个功能查看他所有的父级关系。
6、支持代码重构，方便修改代码。
7、拥有本地历史记录功能（local history功能）。
8、方便的部署，可以直接将代码直接upload到服务器。

**激活方法**

**方法1、**PhpStorm已经升级到2017.1，原注册码失效，2017.1.2注册方法：
注册时选择“License server”输入 
[地址一](http://idea.lanyus.com/)（已被封杀）
或者：
[地址二](http://idea.qinxi1992.cn/)
点击“OK”快速激活JetBrains系列产品


![](http://upload-images.jianshu.io/upload_images/6673460-de6000b265d59c98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**方法2、**直接用浏览器打开 [网址](http://idea.lanyus.com/) ，点击页面中的“获得注册码”，然后在注册时切换至Activation Code选项，输入获得的注册码一长串字符串，便可以注册成功了！（推荐用这种方式）
特别注意：为避免PhpStorm联网时注册失效，请将“0.0.0.0 account.jetbrains.com”添加到hosts文件中。

hosts文件的路径：C:\Windows\System32\drivers\etc


![](http://upload-images.jianshu.io/upload_images/6673460-d58c48983cf701f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](http://upload-images.jianshu.io/upload_images/6673460-20102ce3611ea23b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第二种方法可能会过期，过期之后又得更换注册码，比较麻烦，试试第三种方法；

**方法3、**在文章开头有下载激活文件链接

**破解步骤：**

1、将JetbrainsCrack-2.6.2.jar放到PhpStorm 安装目录下的lib文件夹；
2、找到PhpStorm 的安装路径, 在\bin目录下有两个文件 PhpStorm.exe.vmoptions、 PhpStorm64.exe.vmoptions 打开文件;
3、最后面加入一行
```
-javaagent:C:\Program Files (x86)\JetBrains\PhpStorm 2016.3.2\lib\JetbrainsCrack-2.6.2.jar
```
后面的路径，根据自己放的位置修改；保存文件，关闭并重新打开phpstorm。

4、打开[网址](http://idea.lanyus.com/getkey?userName=Kobe)
userName可以改，然后生成一个激活码，再打开PhpStorm，选择Activation Code，将验证码粘贴进去 激活...


**汉化方法**

下载汉化包，将resources_cn.jar文件复制到 安装目录下的\lib目录中，重启PhpStorm即可。

**界面设置**

1、当你打开PhpStorm看到这样的界面是不是感觉特别的别扭，感觉特别的low，

![](http://upload-images.jianshu.io/upload_images/6673460-9072afdd1a2ec683.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这时就可以进入编辑器里的：文件->设置->外观行为->外观->主题，选择Darcula黑色主题，点击确定。这时就可以看到类似于sublime编辑器里面的界面，是不是好很多了啊！

![](http://upload-images.jianshu.io/upload_images/6673460-1cb74878154e2cd0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、设置字体的大小：文件->设置->编辑器->颜色和字体->Font，即可设置字体的大小。

3、当我们在分辨率高的屏幕里可以看到界面会有一条提示代码不要超过这条划线的竖线，这样我们在编写代码的时候会感觉很别扭，所以可以进入编辑器的：文件->设置->编辑器->编辑器->外观，将“显示右边距（代码样式选项中配置）”前面的勾去掉就可以了。


![](http://upload-images.jianshu.io/upload_images/6673460-db2a3d064ea78511.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[
PhpStorm激活和汉化文件下载网址](http://pan.baidu.com/s/1nuHF1St)（提取密码：62cg）