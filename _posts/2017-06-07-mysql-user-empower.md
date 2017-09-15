---
layout: post
blog_id: "mysql-user-empower"
title: "MySQL数据库新建用户与授权方法 "
date: 2017-06-07 00:00:00 -0700
tags: Mysql
category: Mysql
summary: MySQL数据库新建用户与授权方法
comments: false
---

[Navicat for mysql 下载地址](http://download.csdn.net/detail/kesixin/9862364)

[putty下载地址](http://download.csdn.net/detail/kesixin/9863326)

一般情况下，修改[MySQL](http://lib.csdn.net/base/mysql)密码，授权，是需要有[mysql](http://lib.csdn.net/base/mysql)里的root权限的。

上篇安装完MySQL[数据库](http://lib.csdn.net/base/mysql)之后我们对root用户修改了密码，之后我用root用户在navicat for mysql数据库管理工具进行远程连接，发现会出现如下错误：

![](http://upload-images.jianshu.io/upload_images/6673460-f27490a0e38dc047.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/6673460-6a22d55aaaecd359.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
因为在安装配置的过程中我们设置root账户默认不开放远程访问权限，所以我的解决方案是在MySQL中新建用户并授权。

<strong>解决方案：</strong>

putty连接服务器进行配置:
```
1.新建用户
//登录mysql
@>mysql -u root -p
@>密码

//创建新用户
mysql> CREATE USER 'username'@'host' IDENTIFIED BY 'password';

说明：username - 你将创建的用户名, host - 指定该用户在哪个主机上可以登陆,如果是本地用户可用localhost, 如果想让该用户可以从任意远程主机登陆,可以使用通配符%. password - 该用户的登陆密码,密码可以为空,如果为空则该用户可以不需要密码登陆服务器. 
例如：mysql>CREATE USER 'user'@'localhost' IDENTIFIED BY '123456'; 
```
```
2.授权
mysql>GRANT ALL PRIVILEGES ON *.* TO 'user'@'localhost' IDENTIFIED BY '123456';
```
授予user用户在所有数据库上的所有权限。
 
如果此时发现给的权限太大了，我们可以只给user用户授予某个数据库的权限，这时需要撤销刚才的授权，
```
mysql>EVOKE ALL PRIVILEGES ON *.* FROM 'user'@'localhost';
mysql>GRANT ALL PRIVILEGES ON wordpress.* TO 'user'@'localhost' IDENTIFIED BY '123456';
```
还可以指定该用户只能执行 select和update命令:
```
mysql>GRANT SELECT, UPDATE ON wordpress.* TO 'user'@'localhost' IDENTIFIED BY '123456';
```
刷新权限:
```
mysql>FLUSH PRIVILEGES;
```
这时候用user用户在navicat中登录的话可以发现有一个数据库wordpress。

![](http://upload-images.jianshu.io/upload_images/6673460-3034603a1c56f686.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
3.删除用户
mysql>DROP USER user@localhost;
可以发现不管是授权，还是撤销授权，都要指定响应的host（即@符号后面的内容），因为以上及格命令实际上都是在操作mysql数据库中的user表;
```