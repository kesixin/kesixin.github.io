---
layout: post
blog_id: "git-use-orders"
title: " gitbash-coding 代码仓库 代码备份 命令使用 "
date: 2017-06-06 00:00:00 -0700
tags: Git
category: Git
summary:  gitbash-coding 代码仓库 代码备份 命令使用
comments: false
---

coding 代码仓库......

[coding官网](https://coding.net/)，注册一个账号，就可以成为普通成员，可以创建2个私有项目，128M [Git](http://lib.csdn.net/base/git) 仓库容量；完善填写个人信息可以成为银牌会员，可以创建5个私有项目，256M [git](http://lib.csdn.net/base/git) 仓库容量；

注册完成后可以创建个人项目，项目分为私有和公开，


![](http://upload-images.jianshu.io/upload_images/6673460-fd717c2370c8864f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


创建完项目就相当于拥有一个代码备份仓库；

下载Gitbush安装包，一直下一步安装即可。[下载地址](https://git-scm.com/download)
完成后，打开GIt Bash；

第一步：初始化
在d盘初始化
cd d:
git init
执行完这句命令行后在d盘会出现一个git文件夹，这个文件夹是隐藏文件，一般没有设置显示隐藏文件是看不到的；

第二步：克隆项目
git clone  （仓库地址）

克隆完成之后在目录会出现跟你项目名一样的文件夹，说明你已经克隆成功！

第三步：add暂存文件
当你克隆完项目后需要提交，首先要做的事情是设置你的用户名称和e-mail地址。这是非常重要的，因为每次Git提交都会使用该信息。它被永远的嵌入到了你的提交中：
$ git config --global user.name "wirelessqa"$ git config --global user.email wirelessqa.me@gmail.com
在自己所负责的目录添加代码（把源代码文件复制进来）
git add API   //表示更新API目录下的文件git add ./    //更新根目录git diff API/API.txt//查看更新的内容

第四步：确定更新
git commit -m [Init]//Init为本次更新说明

第五步：推送到远程服务器
git push

第六步：更新
以后每次更新，先将服务器上已经别人新增的内容下载下来git pull