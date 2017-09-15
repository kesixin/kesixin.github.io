---
layout: post
blog_id: "javascript-look-pic"
title: "js实现图片上传预览及进度圆圈 "
date: 2017-08-11 00:00:00 -0700
tags: Javascript
category: Javascript
summary: 最近在做图片上传的时候，上网找了比较久还没有现成的，因此自己做了一个，实现的功能如下
comments: false
---

[完整案例下载地址](http://download.csdn.net/download/kesixin/9929881)

最近在做图片上传的时候，上网找了比较久还没有现成的，因此自己做了一个，实现的功能如下：

1.去除"input file"默认的样式；

2.使用上传可以查看上传进度(本博做的是上传的百分比，做成进度圆圈类似)；

3.图片从本地选择上传后预览图片；

首先是去除浏览器默认上传图片框，通过设置css样式可以简单的修改成想要的效果。

index.html代码如下;

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style type="text/css">
		body{
			text-align: center;
		}
		.uploadWrap{
			margin: auto;
			width: 500px;
			height: 500px;
		}
		#li_idCard{
			    width: 210px;
			    height: 120px;
			    background: #b3dde2;
			    color: #fff;
			    text-align: center;
			    line-height: 120px;
			    font-size: 30px;
			    border: 1px solid #999;
			    position: relative;
				}
		#IDCardFront{
			    width: 210px;
			    height: 120px;
			    opacity: 0;
			    position: absolute;
			    right: 0;
			    top: 0;
			    cursor: pointer;
				}
		.process {
		    width: 280px;
		    height: 140px;
		    position: absolute;
		    top: 30%;
		    left: 40%;
		    display: none;
		}
		#img{
			margin: 0 auto;
			margin-top: 200px;
		}
	</style>
</head>
<body>
	<div class='uploadWrap'>
            <div class="uploadIDcard">
                <div class="upload_box">
                    <ul>
                        <li id='li_idCard'>
                            <span class="percent" id="percent1">+</span>
                            <canvas class="process" id="process1">0%</canvas>
                            <input type="file" id="IDCardFront" name="IDCardFront">
                            <img id='img' src="">
                        </li>
                        
                    </ul>
                </div>
            </div>
            
        </div>
</body>
<script type="text/javascript" src="jquery.min.js"></script>
<script type="text/javascript" src="jquery.form.js"></script>
</html>
```

这里html代码引入外部js文件。

js代码

```
<script>

	//进度条渲染效果
function drawProcess(id) {
    var text = $("#" + id).text();
    var process = text.substring(0, text.length - 1);
    var canvas = $("#" + id);
    var context = canvas[0].getContext('2d');
    context.clearRect(0, 0, 48, 48);
    context.beginPath();
    context.moveTo(240, 240);
    context.arc(24, 24, 24, 0, Math.PI * 2, false);
    context.closePath();
    context.fillStyle = '#fff';
    context.fill();
    context.beginPath();
    context.moveTo(24, 24);
    context.arc(24, 24, 24, 0, Math.PI * 2 * process / 100, false);
    context.closePath();
    context.fillStyle = 'rgb(234,117,96)';  //圈的百分比
    context.fill();
    context.beginPath();
    context.moveTo(24, 24);
    context.arc(24, 24, 21, 0, Math.PI * 2, true);
    context.closePath();
    context.fillStyle = 'rgba(255,255,255,1)';
    context.fill();
    context.beginPath();
    context.arc(24, 24, 18.5, 0, Math.PI * 2, true);
    context.closePath();
    context.strokeStyle = '#ddd';
    context.stroke();
    context.font = "bold 9pt Arial";
    context.fillStyle = 'rgb(234,117,96)';  //百分比数字颜色
    context.textAlign = 'center';
    context.textBaseline = 'middle';
    context.moveTo(24, 24);
    context.fillText(text, 24, 24);
}

$("#IDCardFront").change(function () {

	if ($("#FrontUpload").length > 0) {

    } else {
        $("#li_idCard").wrap("<form id='FrontUpload' action='upload.php' method='post' enctype='multipart/form-data'></form>");
    }

    $("#FrontUpload").ajaxSubmit({
    	
    	dataType: 'json',
        beforeSend: function () {
        
        },
        uploadProgress: function (event, position, total, percentComplete) {
            $('#percent1').hide();
            $('#process1').show();
            $('#process1').text(percentComplete + "%");
            drawProcess('process1');
        },
        success: function (data) {
            if (data.result == 'true') {
                $('#img').attr('src','./'+data.img+'');
            } else {
            	alert('上传失败');
            }
    	}
	});
});
</script>
```
服务器端用的是PHP

upload.php代码：

```
<?php

error_reporting(0);//禁用错误报告

if (file_exists("./" . $_FILES["IDCardFront"]["name"])){
		$arrRet=array(
			'result'=>'false'
		);
}else{
		$ret=move_uploaded_file($_FILES["IDCardFront"]["tmp_name"],"./" . $_FILES["IDCardFront"]["name"]);
		if($ret){
		      $arrRet=array(
				'result'=>'true',
				'img'=> $_FILES["IDCardFront"]["name"]
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

实现效果：

![](http://upload-images.jianshu.io/upload_images/6673460-8633b6a82405b6fc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)