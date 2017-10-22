---
   layout: post
   blog_id: "is-weixin-browser"
   title: "判断是否为微信内置浏览器 "
   date: 2017-10-16 00:00:00 -0700
   tags: PHP
   category: PHP
   summary: 有时候在开发项目我们需要知道当前的浏览器是否为微信内置的浏览器，从而来进行一些相对的操作。如何判断微信内置浏览器，首先需要获取微浏览器的User Agent
   comments: false
---

有时候在开发项目我们需要知道当前的浏览器是否为微信内置的浏览器，从而来进行一些相对的操作。如何判断微信内置浏览器，首先需要获取微浏览器的User Agent。

####   通过PHP判断

在PHP中我们通过$_SERVER["HTTP_USER_AGENT"]数组来获取User Agent。HTTP_USER_AGENT是用来获取用户相关信息，包括了用户浏览器，操作系统工程，是否安装了alex toolbar等信息。


```
function is_weixin(){ 

    if ( strpos($_SERVER['HTTP_USER_AGENT'], 'MicroMessenger') !== false ) {

            echo "微信浏览器";

    }    

    echo "不是微信浏览器";

}


```
<br>

####  通过JavaScript判断


```
function is_weixin() { 
	 //userAgent 属性是一个只读的字符串，声明了浏览器用于 HTTP 请求的用户代理头的值。

	 var ua = window.navigator.userAgent.toLowerCase(); 
	 
	 if (ua.match(/MicroMessenger/i) == 'micromessenger') { 
	        alert("微信浏览器"); 
	    } else { 
	        alert("不是微信浏览器"); 
	    } 
	}
```