---
layout: post
blog_id: "linux-apache-and-tomcat"
title: "Linux系统Apache2.4+Tomcat7.0整合配置 "
date: 2017-09-01 00:00:00 -0700
tags: Apache
category: Apache
summary: 最近在服务器上进行了Apache和Tomcat的整合，让用户对jsp等页面发起的访问可以转交给Tomcat处理，使得Apache服务器支持jsp页面的访问。本文将详细讲述在linux环境下配置。
comments: false
---

##一、安装Apache
之前发的博文有关于安装和配置Apache服务器。

##二、Tomcat的安装配置

1、下载对应的jdk,并配置java环境，我下载的版本是jdk-8u144-linux-x64.rpm。


下载Tomcat的安装包，我下载的版本是apache-tomcat-7.0.81。


```
#rpm -ivh jdk-8u144-linux-x64.rpm
```
安装结束后，jdk会安装在/usr/java里，然后配置环境变量，在 /etc/profile 中加入以下内容：
```
 vi /etc/profile      //进入profile文件加入以下内容：

export JAVA_HOME=/usr/java/jdk1.8.0_144  
export JRE_HOME=/usr/java/jdk1.8.0_144/jre  
exportCLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib  
export PATH=$PATH:$JAVA_HOME/bin  

 source /etc/profile   //使配置生效

 java -version   //测试jdk是否安装成功
```



3、Tomcat的安装

* 解压安装包
```
# mkdir /usr/local/tomcat
# cd /usr/local/tomcat
# tar -zxvf /software/apache-tomcat-7.0.81.tar.gz
```
* 启动Tomcat
```
 # cd /usr/local/tomcat/bin
 # ./startup.sh
```


* 如果出现  " Using_JRE_HOME：/usr/   "这种情况的，那可能是没有执行 " #  source /etc/profile "这句命令。
或者可以在" /usr/local/apache-tomcat-7.0.81/bin "文件夹下的setclasspath.sh文件的开头加入下面的内容：
```
export JAVA_HOME=/usr/java/jdk1.8.0_144  
export JRE_HOME=/usr/java/jdk1.8.0_144/jre
```

* 打开防火墙,使外部能访问
```
 # /sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
 # service iptables save
 # service iptables restart
```
* 如果是阿里云的ecs服务器可以配置添加安全组规则，


##三、编译生成mod_jk.so文件

1、下载tomcat-conntector压缩包，我下载的版本是tomcat-connectors-1.2.42-src.tar.gz，这个要跟安装的Apache版本匹配，我的Apache服务器版本是Apache/2.4.6。

2、下载后，首先把tomcat-connectors-1.2.42-src.tar.gz复制到/usr/local
```
# cp tomcat-connectors-1.2.42-src.tar.gz /usr/local
```
* 解压
```
# tar -xzvf tomcat-connectors-1.2.42-src.tar.gz /usr/local
```
* 解压后，进入native文件夹
```
# cd /usr/local/tomcat-connectors-1.2.42-src/native
# ./configure --with-apxs=/usr/bin/apxs(此处apxs地址可能不一样，可用命令" whereis apxs "来查看)
# make
```
* 完成编译后，在/usr/local/tomcat-connectors-1.2.42-src/native/apache-2.0文件夹下可以找到mod_jk.so文件。将这个文件复制到apache安装路径下的modules文件夹下，我是用yum安装的，所以我的apache默认安装路径是/usr/lib64/httpd。到这里也就完成了编译的步骤。

3、问题：在编译mod_jk.so文件的时候，我遇到了找不到apxs文件，所以编译不了，apxs是一个为Apache HTTP服务器编译和安装扩展模块的工具，用于编译一个或多个源程序或目标代码文件为动态共享对象，使之可以用由mod_so提供的LoadModule指令在运行时加载到Apache服务器中。

（1） 检查 apxs 有没有安装。" # whereis apxs "
（2） 如果没有的话，先安装apxs
```
# cd /etc/
# vi yum.conf
// 如果有关于 apache or httpd 的 "exclude"这样一行，把它注释掉；如果没有，就直接退出就行
// 保存并退出
# yum install apr-util-devel
# yum install httpd-devel
# whereis apxs
// 做完这几步以后，你就应该有 "/usr/bin/apxs" 这个文件了。
        
```
##四、整合Apache和Tomcat

1、创建相关配置文件
* 进入apache安装路径下的conf文件夹，创建两个文件，mod_jk.conf以及workers.properties。

mod_jk.conf 内容如下：
```
#LoadModule jk_module modules/mod_jk.so  
  
JkWorkersFile /etc/httpd/conf/workers.properties  
# Where to put jk logs  
JkLogFile /var/log/httpd/mod_jk.log  
  
# Set the jk log level [debug/error/info]  
JkLogLevel info  
  
# Select the log format  
JkLogStampFormat "[%a %b %d %H:%M:%S %Y]"  
  
# JkOptions indicate to send SSL KEY SIZE,   
JkOptions +ForwardKeySize +ForwardURICompat -ForwardDirectories  
# JkRequestLogFormat set the request format   
JkRequestLogFormat "%w %V %T"  
  
JkMount /*/servlet/*  worker1  
JkMount /*.jsp worker1  
JkMount /application/* worker1  
JkMount /*.do worker1  
JkMount /*.class worker1  
JkMount /*.jar worker1  
```

* 其中，前两个参数（JkWorkerFile和JkLogFile）具体的值会因为apache安装路径的不同而不同，此处需写入自己的apache服务器的路径。
文件末尾的几行JkMount意思是将符合条件的文件交给Tomcat处理。中间则是一些常规参数的设置。

workers.properties内容如下
```
# Defining a worker named worker1 and of type ajp13  
worker.list=worker1  
  
# Set properties for worker1  
worker.worker1.type=ajp13    
worker.worker1.host=localhost    
worker.worker1.port=8009  
worker.worker1.lbfactor=50    
worker.worker1.cachesize=10    
worker.worker1.cache_timeout=600    
worker.worker1.socket_keepalive=1    
worker.worker1.socket_timeout=300
```

* 接着是对/conf文件夹下自带的httpd.conf文件加入如下内容：
```
LoadModule jk_module modules/mod_jk.so
Include /etc/httpd/conf/mod_jk.conf
<VirtualHost *:80>
     DocumentRoot "/usr/local/apache-tomcat-7.0.81/webapps/test"
     JkMount /* worker1
</VirtualHost>
```

* 保存之后重启Apache和Tomcat。

* 如果出现Apache启动报错，那可能是mod_jk.so文件的版本跟Apache版本不符合，需要重新下载，编译。

测试成功：
![](http://upload-images.jianshu.io/upload_images/6673460-d094c61ab287b161.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
