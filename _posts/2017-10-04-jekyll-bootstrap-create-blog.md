---
   layout: post
   blog_id: "jekyll-bootstrap-create-blog"
   title: "jekyll bootstrap搭建github blog "
   date: 2017-10-04 00:00:00 -0700
   tags: Jekyll
   category: Jekyll
   summary: jekyll bootstrap搭建github blog，前提必须有一个GitHub账号且本机安装有Git
   comments: false
---

####  前提 

* 必须有一个GitHub账号且本机安装有Git

####  一、创建一个新的仓库

*  在你的 [github官网](https://github.com) 主页新建一个仓库 名字为USERNAME.github.com USERNAME为你的用户名(下同)

####  二、安装Jekyll-Bootstrap

*  在Git Bash中输入如下命令 将代码clone到你本地

```
git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com  
cd USERNAME.github.com  
git remote set-url origin https://github.com/itmyline/USERNAME.github.com.git  
git push origin master  
```

*  PS:如果想clone到指定目录,则使用如下格式 git clone xxx.git “指定目录”

####  三、完成

*  接下来Github将创建你的公开博客。


![](http://upload-images.jianshu.io/upload_images/6673460-a354190b5e708da9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####   如果你已经有blog在Github上？

*  假设你机器上安装有jekyll,如果没有请参考[Windows上安装Jekyll](http://blog.kesixin.xin/2017/09/jekyll-installed-on-windows)。 在本地运行Jekyll-Bootstrap,打开命令行工具 输入如下命令:


```
$ git clone https://github.com/plusjade/jekyll-bootstrap.git  
$ cd jekyll-bootstrap  
$ jekyll serve  
```

* 浏览器中打开localhost:4000 我们将能看到和搭建在github上一样的效果

文章参考来源：[麦田技术博客](http://blog.itmyhome.com/2015/01/jekyll-bootstrap-build-github-blog)