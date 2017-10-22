---
   layout: post
   blog_id: "centos-use-yum-install-mysql"
   title: "CentOS 7.3下使用yum安装MySQL "
   date: 2017-10-21 00:00:00 -0700
   tags: Mysql
   category: Mysql
   summary: 最近在阿里云ecs买个服务器，想要安装mysql数据库，结果一直安装不成功，之前是在CentOS 6下安装成功的。后来才知道CentOS 7的yum源中默认好像是没有mysql的。为了解决这个问题，要先下载mysql的repo源
   comments: false
---

最近在阿里云ecs买个服务器，想要安装mysql数据库，结果一直安装不成功，之前是在CentOS 6下安装成功的。后来才知道CentOS 7的yum源中默认好像是没有mysql的。为了解决这个问题，要先下载mysql的repo源。

* 1.下载mysql的repo源

```
$ wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
```

* 2.安装mysql-community-release-el7-5.noarch.rpm包

```
$ sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
```

![](http://upload-images.jianshu.io/upload_images/6673460-32f28e6b3a26fa2f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*  安装这个包后，会获得两个mysql的yum repo源：mysql-community.repo，mysql-community-source.repo。

*  3.安装mysql

```
$ sudo yum install mysql-server
```

根据提示安装就可以了,不过安装完成后没有密码,需要重置密码


*  4.重置mysql密码

```
$ mysql -u root
```

登录时有可能报这样的错：ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：

```
$ sudo chown -R root:root /var/lib/mysql
```

* 5.重启mysql服务

```
$ service mysqld restart
```

接下来登录重置密码：

```
$ mysql -u root  //直接回车进入mysql控制台
mysql > use mysql;
mysql > update user set password=password('123456') where user='root';
```