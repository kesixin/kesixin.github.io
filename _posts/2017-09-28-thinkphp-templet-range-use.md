---
   layout: post
   blog_id: "thinkphp-templet-range-use"
   title: "ThinkPHP模板范围判断标签使用 "
   date: 2017-09-29 00:00:00 -0700
   tags: PHP
   category: PHP
   summary: ThinkPHP模板范围判断标签使用
   comments: false
---

* 范围判断标签包括in notin between notbetween四个标签，都用于判断变量是否中某个范围。

#### IN和NOTIN标签

* 用法： 假设我们在控制器中给id赋值为1，例如：

```
$id = 1;
$this->assign('id',$id);
```
* 我们可以使用in标签来判断模板变量是否在某个范围内，例如：

```
<in name="id" value="1,2,3">
id在范围内
</in>

最后会输出：id在范围内。
```

* 如果判断不在某个范围内，可以使用： id不在范围内 可以把上面两个标签合并成为：


```
<in name="id" value="1,2,3">
id在范围内
<else/>
id不在范围内
</in>
```

* name属性还可以支持直接判断系统变量，例如：

```
<in name="Think.get.id" value="1,2,3">
$_GET['id'] 在范围内
</in>
```

* value属性也可以使用变量，例如：

```
<in name="id" value="$range">
id在范围内
</in>
```
* $range变量可以是数组，也可以是以逗号分隔的字符串。

#### BETWEEN 和 NOTBETWEEN

* 可以使用between标签来判断变量是否在某个区间范围内，可以使用：

```
<between name="id" value="1,10">
输出内容1
</between>
```

* 同样，可以使用notbetween标签来判断变量不在某个范围内：

```
<notbetween name="id" value="1,10">
输出内容2
</notbetween>
```

* 当使用between标签的时候，value只需要一个区间范围，也就是只支持两个值，后面的值无效，例如

```
<between name="id" value="1,3,10">
输出内容1
</between>
```

* 实际判断的范围区间是1-3，而不是1-10，也可以支持字符串判断，例如：

```
<between name="id" value="A,Z">
输出内容1
</between>
```

* name属性可以直接使用系统变量，例如：

```
<between name="Think.post.id" value="1,5">
输出内容1
</between>
```

* value属性也可以使用变量，例如：

```
<between name="id" value="$range">
输出内容1
</between>
```

* 变量的值可以是字符串或者数组，还可以支持系统变量。

```
<between name="id" value="$Think.get.range">
输出内容1
</between>
```

#### RANGE

* 也可以直接使用range标签，替换前面的判断用法：

```
<range name="id" value="1,2,3" type="in">
输出内容1
</range>
```

* 其中type属性的值可以用in/notin/between/notbetween，其它属性的用法和IN或者BETWEEN一致。