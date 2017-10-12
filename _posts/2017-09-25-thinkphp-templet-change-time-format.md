---
layout: post
blog_id: thinkphp-templet-change-time-format"
title: "thinkphp模板如何转换时间格式 "
date: 2017-09-25 00:00:00 -0700
tags: PHP
category: PHP
summary: thinkphp模板如何转换时间格式
comments: false
---

```
<!-- 如果有日期输出，即$data.time不为空且不为0，则格式化时间戳，
否则默认当前时间戳，并格式化成日期格式 -->   
{$data.time|default=time()|date='Y-m-d',###}

<!--转换为时间戳格式-->
{'2014-09-02'|strtotime}

<!--将时间转换为时间戳在转换为自己想要的格式-->
{'2014-09-02 12:59:12'|strtotime|date='Y-m-d',###}

```