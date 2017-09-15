---
layout: post
blog_id: "php-phpmailer-sendemail"
title: "PHP中利用PHPMailer实现发邮件 "
date: 2017-07-08 00:00:00 -0700
tags: PHP
category: PHP
summary: PHPMailer是一个用于发送电子邮件的PHP函数包
comments: false
---

[PHPMailer的下载地址](http://download.csdn.net/detail/kesixin/9892738)

**下面以QQ邮箱为例，按照这四个方面来介绍PHPMaIiler的使用：**
 - PHPMailer的介绍
 - 步骤一：使QQ邮箱能够发送邮件
 - 步骤二：使PHP能够使用QQ邮箱发送邮件
 - 步骤三：编写发送邮件代码
 - ThinkPHP使用PHPMailer 发送邮件

**PHPMailer的介绍**

  可运行在任何平台之上；
  支持SMTP验证；
  发送邮时指定多个收件人，抄送地址，暗送地址和回复地址；注：添加抄送、暗送仅win平台下smtp方式支持；
  支持多种邮件编码包括：8bit，base64，binary和quoted-printable；
  自定义邮件头信息，这跟php中通过header函数发送头信息类似支持将邮件正文制作成HTMl内容，那么就可以在邮件正文中插入图片；
  经测试兼容的SMTP服务器包括：Sendmail,qmail,Postfix,Imail,Exchange等。

**步骤一：使QQ邮箱能够发送邮件**

我们的邮箱本来可以发送邮件，但是要实现在我们的网站中发送邮件，那就要设置一下我们的QQ邮箱了，因为此时我们的网站现在是作为一个第三方客户端存在的，所以需要用到的是SMTP服务器来发送，在这里建议把前面的两项开启了！

进入QQ邮箱->点击设置->点击账户

![](http://upload-images.jianshu.io/upload_images/6673460-bc5f2a59e4951b27.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当你点击开启的时候，它会提示：

![](http://upload-images.jianshu.io/upload_images/6673460-aca59bd78616ce39.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当你完成以上步骤之后，就会得到一个授权码，你可以先复制出来，待会我们会用到（开启两项的话会得到两个授权码，一定要最新的！）。

**步骤二：使PHP能够使用QQ邮箱发送邮件**

PHPMailer需要PHP的socket扩展支持，而PHPMailer链接qq域名邮箱时需要ssl加密方式，还得PHP的openssl扩展支持，可以使用phpinfo查看是否开启扩展。


![20160601000527591.jpg](http://upload-images.jianshu.io/upload_images/6673460-5c24f0941aa40387.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如未开启，到PHP安装目录找到php.ini中开启两个扩展支持。

![1499511572(1).jpg](http://upload-images.jianshu.io/upload_images/6673460-4a70f786c7de5f38.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**步骤三：编写发送邮件代码**

index.html代码如下：

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<form action="./index.php" method="post" >
    邮箱：<input  type="text" id="mail" name="mail"/>
    标题：<input  type="text" id="title" name="title"/>
    内容<input  type="text" id="content" name="content"/>
    <input class="button" type="submit" value="发送" style="margin: 0 auto;display: block;"/>
</form>
</body>
</html>
```

封装一个公共的方法（写在 functions.php 文件中）：
```
/**
 *发送邮件方法
 *@param $to：接收者 $title：标题 $content：邮件内容
 *@return bool true:发送成功 false:发送失败
 */

function sendMail($to,$title,$content){

    require_once("phpmailer/class.phpmailer.php"); 
    require_once("phpmailer/class.smtp.php");
    //实例化PHPMailer核心类
    $mail = new PHPMailer();

    //使用smtp鉴权方式发送邮件
    $mail->isSMTP();

    //smtp需要鉴权 这个必须是true
    $mail->SMTPAuth=true;

    //链接qq域名邮箱的服务器地址
    $mail->Host = 'smtp.qq.com';

    //设置使用ssl加密方式登录鉴权
    $mail->SMTPSecure = 'ssl';

    //设置ssl连接smtp服务器的远程服务器端口号，以前的默认是25，但是现在新的好像已经不可用了 可选465或587
    $mail->Port = 465;

    //设置发件人的主机域 可有可无 默认为localhost 内容任意，建议使用你的域名
    $mail->Hostname = 'http://www.lsgogroup.com';

    //设置发送的邮件的编码 可选GB2312 我喜欢utf-8 据说utf8在某些客户端收信下会乱码
    $mail->CharSet = 'UTF-8';

    //设置发件人姓名（昵称） 任意内容，显示在收件人邮件的发件人邮箱地址前的发件人姓名
    $mail->FromName = '发件人姓名（昵称）';

    //smtp登录的账号 这里填入字符串格式的qq号即可
    $mail->Username ='12345678@qq.com';

    //smtp登录的密码 使用生成的授权码（就刚才保存的最新的授权码）
    $mail->Password = '最新的授权码';

    //设置发件人邮箱地址 这里填入上述提到的“发件人邮箱”
    $mail->From = '12345678@qq.com';

    //邮件正文是否为html编码 注意此处是一个方法 不再是属性 true或false
    $mail->isHTML(true); 

    //设置收件人邮箱地址 该方法有两个参数 第一个参数为收件人邮箱地址 第二参数为给该地址设置的昵称 不同的邮箱系统会自动进行处理变动 这里第二个参数的意义不大
    $mail->addAddress($to,'尊敬的客户');

    //添加多个收件人 则多次调用方法即可
    // $mail->addAddress('xxx@163.com','尊敬的客户');

    //添加该邮件的主题
    $mail->Subject = $title;

    //添加邮件正文 上方将isHTML设置成了true，则可以是完整的html字符串
    $mail->Body = $content;

    $status = $mail->send();

    //判断与提示信息
    if($status) {
        return true;
    }else{
        return false;
    }
}
```
index.php代码如下：
```
<?php
require_once("./functions.php");
$to=$_POST['mail'];
$title=$_POST['title'];
$content=$_POST['content'];
$flag = sendMail($to,$title,$content);
if($flag){
    echo "发送邮件成功！";
}else{
    echo "发送邮件失败！";
}
?>
```

如果你使用的是QQ企业邮箱，那么链接qq域名邮箱的服务器地址和smtp登录的密码就不同了:
```
//链接qq域名邮箱的服务器地址
$mail->Host = 'smtp.exmail.qq.com';
//smtp登录的密码 （QQ企业邮箱的登录密码）
$mail->Password = '登录密码';
```
**ThinkPHP使用PHPMailer 发送邮件**

PHPMailer解压到ThinkPHP\Library\Vendor

在Common文件夹新建function.php
```
/**
 * 邮件发送函数
 * @param $to：接收者 $title：标题 $content：邮件内容
 * @return bool true:发送成功 false:发送失败
 */
function sendMail($to, $title, $content) {

    Vendor('PHPMailer.PHPMailerAutoload');
    Vendor('PHPMailer.class.smtp');
    $mail = new PHPMailer(); //实例化
    $mail->IsSMTP(); // 启用SMTP
    $mail->Host=C('MAIL_HOST'); //smtp服务器的名称
    $mail->SMTPSecure = C('MAIL_SECURE');
    $mail->Port = C('MAIL_PORT');
    $mail->SMTPAuth = C('MAIL_SMTPAUTH'); //启用smtp认证
    $mail->Username = C('MAIL_USERNAME'); //你的邮箱名
    $mail->Password = C('MAIL_PASSWORD') ; //邮箱密码
    $mail->From = C('MAIL_FROM'); //发件人地址（也就是你的邮箱地址）
    $mail->FromName = C('MAIL_FROMNAME'); //发件人姓名
    $mail->AddAddress($to,"尊敬的客户");
    $mail->WordWrap = 50; //设置每行字符长度
    $mail->IsHTML(C('MAIL_ISHTML')); // 是否HTML格式邮件
    $mail->CharSet=C('MAIL_CHARSET'); //设置邮件编码
    $mail->Subject =$title; //邮件主题
    $mail->Body = $content; //邮件内容
    $mail->AltBody = "您好"; //邮件正文不支持HTML的备用显示
    return($mail->Send());
}
```
添加配置文件config.php
```
// 配置邮件发送服务器
    'MAIL_HOST' =>'smtp.qq.com',//smtp服务器的名称
    'MAIL_SMTPAUTH' =>true, //启用smtp认证
    'MAIL_USERNAME' =>'12345678@qq.com',//你的邮箱名
    'MAIL_FROM' =>'12345678@qq.com',//发件人地址
    'MAIL_FROMNAME'=>'12345678@qq.com',//发件人姓名
    'MAIL_PASSWORD' =>'xxxxxx,//邮箱密码
    'MAIL_CHARSET' =>'utf-8',//设置邮件编码
    'MAIL_ISHTML' =>TRUE, // 是否HTML格式邮件
    'MAIL_PORT' =>'465',//设置ssl连接smtp服务器的远程服务器端口号
    'MAIL_SECURE' =>'ssl',//设置使用ssl加密方式登录鉴权
```

最后就是使用PHPMailer发送邮件
```
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<form action="/index.php/Admin/test/add" method="post" enctype="multipart/form-data">
    邮箱：<input  type="text" id="mail" name="mail"/>
    标题：<input  type="text" id="title" name="title"/>
    内容<input  type="text" id="content" name="content"/>
    <input class="button" type="submit" value="发送" style="margin: 0 auto;display: block;"/>
</form>
</body>
</html>
```
```
public function add(){
        if(sendMail($_POST['mail'],$_POST['title'],$_POST['content']))
            echo "发送成功";
        else
            echo "发送失败";
    }
```