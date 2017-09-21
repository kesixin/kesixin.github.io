---
layout: post
blog_id: js-get-url-param"
title: "用JS获取地址栏参数的方法 "
date: 2017-09-20 00:00:00 -0700
tags: Javascript
category: Javascript
summary: 用js简单获取url或者url字符串中的参数的值
comments: false
---

#### 直接用window.location获取
```
//@param name string url中的参数名

function GetQueryString(name) {

    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)","i");

    var r = window.location.search.substr(1).match(reg);

    if (r!=null) return unescape(r[2]); return null;

}

// 调用方法
alert(GetQueryString("参数名1"));
alert(GetQueryString("参数名2"));
alert(GetQueryString("参数名3"));
```

* 下面举个例子：
若地址栏URL为：abc.html?id=666
如果用：alert(GetQueryString("id"));那么弹出的内容就是 "666" 啦；
当然如果你没有传参数的话，比如你的地址是 abc.html 后面没有参数，那强行输出调用结果有的时候会报错：
所以我们要加一个判断 ，判断我们请求的参数是否为空，首先把值赋给一个变量：

```
var myurl=GetQueryString("url");
if(myurl !=null && myurl.toString().length>1)
{
   alert(GetQueryString("url"));
}

```
这样就不会报错了！


#### 字符串url获取

```
<script type="text/javascript">
	var str="www.kesixin.xin/index.html?name=aaa&sex=bbb";
	var i=str.indexOf('?');
	alert(GetQueryString(str.substr(i),"name"));

	function GetQueryString(value,name) {

	        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)","i");

	        var r = value.substr(1).match(reg);

	        if (r!=null) return unescape(r[2]); return null;

		}
</script>
```