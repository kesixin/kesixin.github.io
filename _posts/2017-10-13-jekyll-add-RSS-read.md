---
   layout: post
   blog_id: "jekyll-add-RSS-read"
   title: "Jekyll博客添加RSS feed订阅功能 "
   date: 2017-10-13 00:00:00 -0700
   tags: Jekyll
   category: Jekyll
   summary: 简易信息聚合（也叫聚合内容）是一种RSS基于XML标准，在互联网上被广泛采用的内容包装和投递协议。RSS(Really Simple Syndication)是一种描述和同步网站内容的格式，是使用最广泛的XML应用
   comments: false
---


简易信息聚合（也叫聚合内容）是一种RSS基于XML标准，在互联网上被广泛采用的内容包装和投递协议。RSS(Really Simple Syndication)是一种描述和同步网站内容的格式，是使用最广泛的XML应用。RSS搭建了信息迅速传播的一个技术平台，使得每个人都成为潜在的信息提供者。发布一个RSS文件后，这个RSS Feed中包含的信息就能直接被其他站点调用，而且由于这些数据都是标准的XML格式，所以也能在其他的终端和服务中使用，是一种描述和同步网站内容的格式。


RSS目前广泛用于网上新闻频道，blog和wiki，主要的版本有0.91, 1.0, 2.0。使用RSS订阅能更快地获取信息，网站提供RSS输出，有利于让用户获取网站内容的最新更新。网络用户可以在客户端借助于支持RSS的聚合工具软件，在不打开网站内容页面的情况下阅读支持RSS输出的网站内容。接下来讲述一下在jekyll博客添加RSS feed订阅功能。



####   1、在_config.yml文件 添加下列属性：


```
name:         blog Name  
description:  A description for your blog  
url:          http://your-blog-url.com  
```


* {% raw %}这些值{{ site.name }}，{{ site.description }}，{{ site.url }}会在你的feed文件里用到。{% endraw %}
<br>


####  2、在网站根目录下添加 feed.xml


```
{% raw %}

---
layout: none
---

<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" title="XSL Formatting" href="/rss.xsl" media="all" ?>

<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
	<channel>
		<title>{{ site.name }}</title>
		<description>{{ site.description }}</description>
		<link>{{ site.baseurl}}{{ site.url }}</link>
		<atom:link href="{{ site.baseurl}}{{ site.url }}/feed.xml" rel="self" type="application/rss+xml" />
		{% for post in site.posts %}
			<item>
				<title>{{ post.title }}</title>
				<description>{{ post.content | xml_escape }}</description>
				<pubDate>{{ post.date | date: "%a, %d %b %Y %H:%M:%S %z" }}</pubDate>
				<link>{{ site.url }}{{ site.baseurl}}{{ post.url }}</link>
				<guid isPermaLink="true">{{ site.url }}{{ site.baseurl}}{{ post.url }}</guid>
			</item>
		{% endfor %}
	</channel>
</rss>

{% endraw %}

```

*   在你网站的合适地方添加如下代码：

```
{% raw %}
<a href="{{ site.url }}/feed.xml">RSS订阅</a>  
{% endraw %}
```


rss.xsl文件下载地址：[点击下载](http://download.csdn.net/download/kesixin/10026173)


文章参考来源：[麦田技术博客](http://blog.itmyhome.com/2015/01/add-rss-feed-subscriptions-for-jekyll-blog)