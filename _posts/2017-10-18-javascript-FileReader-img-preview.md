---
   layout: post
   blog_id: "javascript-FileReader-img-preview"
   title: "JavaScript使用FileReader对象实现图片上传预览 "
   date: 2017-10-18 00:00:00 -0700
   tags: Javascript
   category: Javascript
   summary: 一个简单的实例，JavaScript实现图片上传预览
   comments: false
---


一个简单的实例，JavaScript实现图片上传预览。


代码如下：


```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<input type='file' onchange='openFile(event)'><br>
	<img id='output'>
</body>

<script type="text/javascript">

	var openFile = function(event) {

		//target 事件属性可返回事件的目标节点（触发该事件的节点），如生成事件的元素、文档或窗口。
		//IE下,event对象有srcElement属性,但是没有target属性;

		//Firefox下,event对象有target属性,但是没有srcElement属性.但他们的作用是相当的
		
	    var input = event.target;

	    var reader = new FileReader();
		//将文件读取为一段以 data: 开头的字符串
	    reader.readAsDataURL(input.files[0]);
		
	    reader.onload = function(){
	      var dataURL = this.result;
	      var output = document.getElementById('output');
	      output.src = dataURL;
	    };
		

  };
</script>
</html>
```


<br>

![](http://img.blog.csdn.net/20171022201831282?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2VzaXhpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


![](http://img.blog.csdn.net/20171022202913428?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQva2VzaXhpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)