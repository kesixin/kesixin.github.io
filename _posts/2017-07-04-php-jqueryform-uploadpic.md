---
layout: post
blog_id: "php-jqueryform-uploadpic"
title: "php使用jquery Form 实现页面无刷新上传图片，并预览图片 "
date: 2017-07-04 00:00:00 -0700
tags: PHP
category: PHP
summary: php使用jquery Form 实现页面无刷新上传图片，并预览图片
comments: false
---

一、jquery.form.js下载地址

[jquery.form.js下载地址](http://download.csdn.net/detail/kesixin/9863215)

<br>
二、例子如下：

demo.html代码如下：

```
<!DOCTYPE HTML>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>ajax表单处理</title>
</head>
<body>
	上传图片：<input type="file" name="banner" id="banner"/>
	预览图片：<img src="" id="banner_see">
</body>
<script type="text/javascript" src="jquery.min.js"></script>
<script type="text/javascript" src="jquery.form.js"></script>
<script type="text/javascript">
	$('#banner').on('change', function () {
		if ($("#mbanner").length > 0) {

        } else {
	            $("#banner").wrap("<form id='mbanner' action='demo.php' method='post' xenctype='multipart/form-data'></form>");
                }
       /*ajax提交*/
        $("#mbanner").ajaxSubmit({
              dataType: 'json',
              beforeSend: function () {

              },
              uploadProgress: function (event, position, total, percentComplete) {

              },
              success: function (data) {
                  if (data.result == 'true') {
                       $('#banner_see').attr('src',data.img);
                  } else {
                       $('#banner').val("");
                 }
             },
             error: function (xhr) {
                        
             }
        });
   });
</script>
```

demo.php代码如下：

```
<?php
	if (file_exists("./" . $_FILES["banner"]["name"]))
    {
	  $arrRet=array(
		'result'=>'false'
		);
      
    }
	else
    {
      $ret=move_uploaded_file($_FILES["banner"]["tmp_name"],"./" . $_FILES["banner"]["name"]);
      if($ret){
	      $arrRet=array(
			'result'=>'true',
			'img'=> $_FILES["banner"]["name"]
			);
		}else{
			$arrRet=array(
				'result'=>'false'
			);
		}
    }
    echo json_encode($arrRet);
?>
```

结果如下图：

![](http://upload-images.jianshu.io/upload_images/6673460-22e273a0e6661e9b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)