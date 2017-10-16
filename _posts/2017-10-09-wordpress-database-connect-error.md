---
   layout: post
   blog_id: "wordpress-database-connect-error"
   title: "WordPress连接数据库出错解决方法 "
   date: 2017-10-09 00:00:00 -0700
   tags: WordPress
   category: WordPress
   summary: Wordpress是一个个人网站（博客）架设平台，我们可以通过它方便的构建属于我们自己的个人网站，但有的时候通过在安装wordpress时可能会出现“数据库连接错误”的问题
   comments: false
---

Wordpress是一个个人网站（博客）架设平台，我们可以通过它方便的构建属于我们自己的个人网站，但有的时候通过在安装wordpress时可能会出现“数据库连接错误”的问题。它可能会极大的困扰我们，多方求助却无结果，那么我们应该怎样解决呢？

#### 1、 数据库配置(wp-config.php)


```
// ** MySQL 设置 - 具体信息来自您正在使用的主机 ** //
/** WordPress数据库的名称 */
define('DB_NAME', 'wordpress');


/** MySQL数据库用户名 */
define('DB_USER', '用户名');


/** MySQL数据库密码 */
define('DB_PASSWORD', '密码');


/** MySQL主机 */
define('DB_HOST', 'localhost');
修改好上面的东西之后，发现wordpress还是不能连接到数据库，最后将localhost改为“127.0.0.1”，即
/** MySQL主机 */
define('DB_HOST', '127.0.0.1');
```


*  再次在浏览器中输入wordpress后台地址域名/wp-admin。

*  为什么会出现这种情况呢，是“localhost”与“127.0.0.1”有什么区别吗？

 谷歌一下，发现了下面的结果：

(1) localhost：不联网，不使用网卡，不受防火墙和网卡限制，本机访问。

(2)127.0.0.1：不联网，网卡传输，受防火墙和网卡限制，本机访问

(3)本机IP，联网，网卡传输 ，受防火墙和网卡限制，本机或外部访问


####  2、需要修复数据库

![](http://upload-images.jianshu.io/upload_images/6673460-3e778d9747d2b76f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*  去wordpress的数据库配置配置文件（wp-config.php）添加如下代码：


```
define('WP_ALLOW_REPATR',true);

```

*  点击刚刚页面的“修复数据库"，完成数据库表的修复，网站就恢复正常了。