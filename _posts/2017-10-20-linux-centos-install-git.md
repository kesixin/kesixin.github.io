---
   layout: post
   blog_id: "linux-centos-install-git"
   title: "Linux-centos系统安装git "
   date: 2017-10-20 00:00:00 -0700
   tags: Git
   category: Git
   summary: Git是在今天的软件开发行业一个非常有用的版本控制工具。如果你曾经使用过Github，那么你可能知道Git是什么。至少，你肯定对它有过了解。Git支持主流的操作系统，如Linux，POSIX，Windows和OS X
   comments: false
---


Git是在今天的软件开发行业一个非常有用的版本控制工具。如果你曾经使用过Github，那么你可能知道Git是什么。至少，你肯定对它有过了解。Git支持主流的操作系统，如Linux，POSIX，Windows和OS X.

####  前期准备

*  请确保您的机器上安装有CentOS系统以及一个帐户具有root权限。因为我们需要在系统上安装软件。


####  安装


*  1.下载git

```
wget https://github.com/git/git/archive/v2.14.1.zip

```

![](http://img.blog.csdn.net/20171022211107102?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2VzaXhpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


*  2.安装依赖

```
sudo yum -y install zlib-devel openssl-devel cpio expat-devel gettext-devel curl-devel perl-ExtUtils-CBuilder perl-ExtUtils-MakeMaker
```

![](http://img.blog.csdn.net/20171022211248537?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2VzaXhpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

*  3.解压git

```
unzip v2.14.1.zip
```


![](http://img.blog.csdn.net/20171022211417023?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2VzaXhpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


*  4.将git安装到/usr/local上

```
//先进入git文件夹
cd git-2.14.1
//编译
make prefix=/usr/local all
//安装
make prefix=/usr/local install
```


*  5.验证是否安装完成


```
git --version
```


![](http://img.blog.csdn.net/20171022212107031?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2VzaXhpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

