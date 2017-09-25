---
   layout: post
   blog_id: "window-apache-and-tomcat"
   title: "Window系统Apache2.4+Tomcat7.0整合配置 "
   date: 2017-08-31 00:00:00 -0700
   tags: Apache
   category: Apache
   summary: 最近在服务器上进行了Apache和Tomcat的整合，让用户对jsp等页面发起的访问可以转交给Tomcat处理，使得Apache服务器支持jsp页面的访问。本文将详细讲述在window环境下配置。
   comments: false
---

#### 一、关于Apache和Tomcat

Apache HTTP Server（简称 Apache），是 Apache 软件基金协会的一个开放源码的网页服务器，可以在 Windows、Unix、Linux 等操作系统中运行是最流行的Web服务器软件之一。Apache 反应速度快，运行效率高，但只支持HTML等静态页面（加载插件后也可支持 PHP 页面）。

Tomcat 是由 Apache 软件基金协会与 Sun 公司联合开发的一款Web服务器，它除了支持HTML等静态页面外，还支持JSP、Servlet 。

在相同的运行环境下，Tomcat 对静态页面的反应速度没有 Apache 灵敏，整合 Apache 与 Tomcat 能使系统运行于一个良好环境下，提高系统效率。

#### 二、运行环境

运行环境：Window10系统，与Window7系统整合是一样的。

软件包：xampp集成软件包，包含了Apache+MySQL+PHP+Tomcat，本文就不介绍Apache和Tomcat服务器的安装和配置了。

mod_jk.so：连接Apache和Tomcat，这里有很多版本，主要是要跟Apache版本能够匹配，不然启动Apazhe服务时会报错，启动不了。


JDK：JDK (Java Development Kit) 是 Sun 针对Java开发员的产品，是现今使用最广泛的Java SDK。JDK 是整个Java的核心，包括了Java运行环境和基础类库等。常用版本包括 JDK 6、JDK 7、JDK 8。

成功安装 JDK  后，必须设置环境变量；

打开 “控制面板 --> 系统安全 --> 系统 --> 高级系统设置 --> 环境变量”

![](http://upload-images.jianshu.io/upload_images/6673460-9e87bcf7a13a0edd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 三、整合 Apache+Tomcat

1、在 Apache、Tomcat 都能正常运行的情况下，把 mod_jk.mo 拷贝到 "D:\xampp\apache\modules" 文件夹下。

2、在 "D:\xampp\tomcat\conf" 文件夹下增加 workers.properties 文件，输入以下内容。

```
workers.tomcat_home=D:\xampp\tomcat
#让 mod_jk 模块感知 Tomcat
workers.java_home=F:\Java\jdk1.7.0_75\jre
#让 mod_jk 模块感知 jre
ps=\
#指定文件路径分割符
worker.list=config1
worker.config1.port=8009
#工作端口，此端口应该与server.xml中Connector元素的 AJP/1.3 协议所使用的端口相匹配
worker.config1.host=test.com
#Tomcat服务器的地址
worker.config1.type=ajp13
#类型
worker.config1.lbfactor=1
#负载平衡因数


注意：
worker.list=config1中，conifg1为自定义名称，但此名称必须与下文所述的 
"D:\xampp\apache\conf\httpd.conf " 文件中 ，JkMount 指令对应的名称相匹配。
```

3、加入workers.properties 文件后，可修改 "D:\xampp\apache\conf\httpd.conf" 文件，加入以下配置。
注意：JkMount 指令中的变量必须与worker.list 所配置的名称相同。
```
# 设置虚拟主机
#本地虚拟测试，需要在"C:\Windows\System32\drivers\etc"中的host文件加入："127.0.0.1    test.com"
 <VirtualHost *:80>
     ServerName test.com
     #定义服务名称
     DocumentRoot "D:\xampp\tomcat\webapps"
     #定义站点项目所在路径，把路径指向 tomcat 中的默认网站目录
     DirectoryIndex index.html index.htm index.jsp
     ErrorLog logs/shsc-error_log.txt
     CustomLog logs/shsc-access_log.txt common
     #向Apache请求此文件夹内页面时，系统将转向用Tomcat解析
     JkMount /* config1     
 </VirtualHost>
 
 # 允许客户端访问此路径
 <Directory "D:\xampp\tomcat\webapps">  
     Options Indexes FollowSymLinks  
     AllowOverride None  
     Order allow,deny  
     Allow from all  
 </Directory>  
 
 LoadModule jk_module modules/mod_jk.so
 # 此处 mod_jk.so 文件为你下载的文件
 JkWorkersFile "D:/xampp/tomcat/conf/workers.properties"
 # 指定tomcat监听配置文件地址
 JkLogFile "D:/xampp/tomcat/logs/mod_jk2.log"
 # 指定日志存放位置
 JkLogLevel info

注意：JkMount /* config1 指令代表当客户端向 Apache 发送此文件夹内 页面请求时，把处理指向
Tomcat。
```
完成以上配置后，重启 Apache、Tomcat。

问题：如果按要求完成上面配置，在重启Apache时出现如下情况：

![](http://upload-images.jianshu.io/upload_images/6673460-ab937b2bd67875b7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


那可能是你的mod_jk.so文件的版本跟Apache服务器版本不匹配，需要重新下载正确的版本，最好是下载最新版本，也不要去下载网上一些mod_jk.so文件，一般很难找到匹配的，而要根据安装的Apache版本去官网下载对应的版本再重新拷贝到modules文件夹中。

测试成功如下图：
![](http://upload-images.jianshu.io/upload_images/6673460-cd17a77628d0f6ff.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好了，Window系统Apache2.4+Tomcat7.0整合配置到此完成，下篇博文将讲解在Linux系统的整合配置。