---
layout: post
blog_id: "thinkphp-uploadify-error"
title: "ThinkPHP 解决使用uploadify 在Firefox浏览器上传图片出现HTTP 302报错 "
date: 2017-06-10 00:00:00 -0700
tags: PHP
category: PHP
summary: ThinkPHP 解决使用uploadify 在Firefox浏览器上传图片出现HTTP 302报错
comments: false
---

[Uploadify 参数说明](http://www.cnblogs.com/yangy608/p/3915349.html)

一、uploadify使用详解

1.在html中的file标签可以用来上传文件，但是在文件较大或者多文件上传的时候，file标签就不太适合了。而uploadify插件是基于[js](http://lib.csdn.net/base/javascript)的[jQuery](http://lib.csdn.net/base/jquery)库写的，结合ajax和flash，实现多文件上传功能。

2.主要文件： 
jquery.uploadify.js （主要插件）
 jquery-1.7.2.min.js （jquery主件）
 uploadify.swf （flash上传插件）
 uploadify.css （上传样式表）
 uploadify-cancel.png （flash上传按钮图标） 
uploadify.php （上传处理数据）
 uploads文件夹 （默认保存上传文件目录）

3.在上传的页面引入js/css文件，然后初始化一些参数变量
```
$(function () {
            $('#file_upload').uploadify({
                buttonText:'选择文件',
                swf: '{$smarty.const.SITE_PUBLIC_URL}Admin/uploadify/uploadify.swf',
                uploader: '{$smarty.const.__CONTROLLER__}/Upload',
                method:'get',
                onSWFReady:function(){
                    $('#file_upload').uploadify('disable', true);

                },
                onUploadStart:function(file){
                    var spot=$('#spot').val();
                    var coid={$arrData['coid']};
                    var session_id={$smarty.session.session_id};
                    $("#file_upload").uploadify("settings", "formData", { 'spot' : spot,'coid':coid,'session_id':session_id});   
                },
                onUploadSuccess:function(file, data, response){
                }
            });
  }); 
```

接着就是在body里面添加调用标签
```
 <p><i>多图上传<span class="must">*</span></i></p>
 <br><br>  
 <input id="file_upload" name="file_upload" type="file" multiple="true">
```
最后就是在后台处理上传的文件。。。。。。

二、出现的问题

1.由于jquery uploadify是借助flash来实现上传的，所以可能在浏览器禁止网站运行flash是会出现如下这个情况：

![](http://upload-images.jianshu.io/upload_images/6673460-3cfdfc4d3213b8bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/6673460-f4a37afd09c16e0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/6673460-1fab6b28c3208e81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
正常的情况：

![](http://upload-images.jianshu.io/upload_images/6673460-d0f3c50666683c0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.Jquery Uploadify在IE浏览器可以正常上传，在Chrome浏览器使用也是正常的，只有在Firefox浏览器使用才会出现HTTP 302报错。

![](http://upload-images.jianshu.io/upload_images/6673460-a8bc436eeb304231.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在实现异步上传的时候，每一个文件在上传时都会提交给服务器一个请求。每个请求都需要安全验证，session和cookie的校验。由于jquery uploadify是借助flash来实现上传的，每一次向后台发送数据流请求时，IE会自动把本地cookie存储捆绑在一起发送给服务器。但是firefox不会这样做，所以在进行验证登录的时候就出现了HTTP 302 报错。如果把这个模块放在不需要验证的地方是不会出现这种报错的。

Session又称为会话状态，是Web系统中最常用的状态，用于维护和当前浏览器实例相关的一些信息。Session对于每一个客户端是不一样的，用户首次与Web服务器建立连接的时候，服务器会给用户分发一个 SessionID作为标识。SessionID是一个由24个字符组成的随机字符串。用户每次提交页面，浏览器都会把这个SessionID包含在 HTTP头中提交给Web服务器，这样Web服务器就能区分当前请求页面的是哪一个客户端。

三.解决问题

1.解决方案一：

在插件初始化的时候，把本地记录下来的session值，以及身份验证值传给初始化方法，进行参数赋值，这样，每次异步请求上传文件的时候，相应的 session值就包含在请求文件中了。
```
onUploadStart:function(file){
      var spot=$('#spot').val();
      var coid={$arrData['coid']};
      var session_id={$smarty.session.session_id};
      $("#file_upload").uploadify("settings", "formData", {'spot' : spot,'coid':coid,'session_id':session_id});   
                },
```
服务器端（ThinkPHP控制器）代码
```
public  function _initialize(){
     //此处为解决Uploadify在火狐下出现http 302错误 重新设置SESSION
    $session_name= session_name();
    if(isset($_GET['session_id']))
     {
        session_id($_GET['session_id']);
        session_start();
      }
       //执行登陆验证检测函数
      $this->powerverify(); 
 }
```
2.解决方案二：
```
onUploadStart:function(file){
      var spot=$('#spot').val();
      var coid={$arrData['coid']};
      var session_id={$smarty.session.session_id};
      $("#file_upload").uploadify("settings", "formData", {'spot' : spot,'coid':coid,'session_id':session_id});   
                },
```
服务器端（ThinkPHP控制器）代码
在核心类文件夹里下的Conf/convention.[php](http://lib.csdn.net/base/php)中 将 VAR_SESSION_ID打开(建议在模块的conf文件中添加配置，如在模块下的Conf/config.php中添加 ‘VAR_SESSION_ID’ => ‘session_id’,)
```
<?php

return array(

    'VAR_SESSION_ID' => 'session_id',//sessionID的提交变量

);
```
在解决了问题之后，我再上Firefox浏览器去上传图片就成功了！

![](http://upload-images.jianshu.io/upload_images/6673460-da94cf162de8ae9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)