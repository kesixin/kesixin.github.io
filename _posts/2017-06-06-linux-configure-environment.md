---
layout: post
blog_id: "linux-configure-environment"
title: " 阿里云ECS服务器Linux环境下配置php运行环境 "
date: 2017-06-06 00:00:00 -0700
tags: Apache
category: Apache
summary:  阿里云ECS服务器Linux环境下配置php运行环境
comments: false
---

阿里云ECS服务器[Linux](http://lib.csdn.net/base/linux)环境安装配置[PHP](http://lib.csdn.net/base/php)的运行环境，不同于window[操作系统](http://lib.csdn.net/base/operatingsystem)下配置，因为是[linux](http://lib.csdn.net/base/linux)操作系统主要是在命令窗体里输入命令来操作，对于初次接触过linux系统的可能会有点怕怕的，下不去手。。。。。。

需要安装的软件有Apache+[php](http://lib.csdn.net/base/php)+[MySQL](http://lib.csdn.net/base/mysql)。

<strong>安装Apache</strong>
```
yum install httpd #根据提示，输入Y安装即可成功安装
```
安装成功后开启Apache，
```
/etc/init.d/httpd start  #启动Apache
```
Apache启动之后可能会提示错误：httpd:httpd: Could not reliably determine the server's fully qualif domain name, using ::1 for ServerName
```
解决方法：vi /etc/httpd/conf/httpd.conf  #编辑http.conf文件
查找到 #ServerName www.example.com:80
按“i”键代表修改文档
修改为 ServerName localhost:80 #这里设置为你自己的域名，如果没有域名，可以设置为localhost
```

![image.png](http://upload-images.jianshu.io/upload_images/6673460-43f9e4ab41b8a165.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
按“Esc”键退出，然后再按“:wq”#保存退出
chkconfig httpd on #设为开机启动
/etc/init.d/httpd restart #重启Apache服务器
```
（如果不是很确定在命令窗体能够修改，可以下载一个WinSCP连接服务器后按 /etc/httpd/conf/httpd.conf这个路径查找修改，代码大概在278行，在修改之前把文件格式先转换为utf-8编码，或者先将httpd.conf 文件备份，防止改错了可以替换回去！！）


![image.png](http://upload-images.jianshu.io/upload_images/6673460-5c9a87a6c74eb439.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第一步已经完成了，哈哈哈哈！


<strong>**安装[mysql](http://lib.csdn.net/base/mysql)**</strong>
```
yum install mysql mysql-server #根据提示，输入Y安装即可成功安装
/etc/init.d/mysqld start #启动MySQL服务
chkconfig mysqld on #设置为开机启动
cp  /usr/share/mysql/my-medium.cnf  /etc/my.cnf  #拷贝配置文件
```
接下来就是为mysql的root账号设置密码（默认的密码是空）
mysql_secure_installation 按回车键
如果你是新安装mysql，会弹出提示：
```
In order to log into MySQL to secure it, we'll need the current password
 for the root user. If you've just installed MySQL, and you haven't set the root password yet, the password will be blank, so you should just press enter here.

Enter
 current password for root (enter for none):
```
因为你是新安装，默认密码为空，直接按enter键就可以。
然后设置新的密码，输入Y即可
输入两次密码确认（一定要记住密码哦！后面设置其他用户还需要用的密码进入MySQL，创建用户等操作）

再接着就是会有若干个提示：
```
By default, a MySQL installation has an anonymous user, allowing anyone to log into MySQL without having to have a user account created for them. This is intended only for testing, and to make the installation [Go](http://lib.csdn.net/base/go) a bit smoother. You should remove them before moving into aproduction environment.
Remove anonymous users? [Y/n] y
MySQL会默认创建一个匿名用户，问你是否删除，一般输入Y删除掉。
```

```
Normally, root should only be allowed to connect from 'localhost'. This ensures that someone cannot guess at the root password from the network.
Disallow root login remotely? [Y/n] y
root用户默认只能访问localhost，以防有人猜密码，是否禁止root登陆，一般选择yes。
```
```
By default, MySQL comes with a database named 'test' that anyone can access. This is also intended only for testing, and should be removed before moving into a production environment.
Remove test database and access to it? [Y/n] 
mysql默认创建一个名为test的[数据库](http://lib.csdn.net/base/mysql)，这个库任何人都可以访问，是否删除掉，一般不删除。
```
```
Reloading
 the privilege tables will ensure that all changes made so far 
will take effect immediately.

Reload
 privilege tables now? [Y/n] 

意思是上面的修改是否马上生效：输入Y
```
```
最后会出现：Thanks
 for using MySQL!
MySQL密码设置完成，重新启动MySQL：
/etc/init.d/mysqld restart #重启MySQL服务
```

<br>
<strong>安装php</strong>
```
yum install php #根据提示，输入Y安装即可成功安装
安装php组件，让php5 支持 mysql
yum install php-mysql php-gd libjpeg* php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-mcrypt php-bcmath php-mhash libmcrypt  #根据提示，输入Y安装即可成功安装
/etc/init.d/mysqld restart #重启mysql
/etc/init.d/httpd restart #重启Apache
```

终于完成了所有软件的安装，现在进行[测试](http://lib.csdn.net/base/softwaretest)一下
安装完成之后在/var/www/ 会有一个html文件夹，这个就是默认的访问路径。
编写一个index.php 文件进行测试
```
<?php
echo "hello world";
?>
```
！！！！！！！！！！！！！！！
如果前面的三个安装步骤都成功，而在测试的时候出现访问不了的问题，有可能是因为阿里云ECS服务器里面的安全组设置问题；
解决方法，在云ECS服务器里添加安全组规则；
这里要登录阿里云的控制台--[控制台](https://www.aliyun.com/)
选择云服务器ECS->安全组，找到你的服务器在哪个区，选择配置规则->添加安全组规则

![image.png](http://upload-images.jianshu.io/upload_images/6673460-82b179388d32b9cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/6673460-cc356ef0b1111470.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)