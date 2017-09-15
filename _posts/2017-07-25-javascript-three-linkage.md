---
layout: post
blog_id: "javascript-three-linkage"
title: "JS全国城市三级联动 "
date: 2017-07-25 00:00:00 -0700
tags: Javascript
category: Javascript
summary: JS全国城市三级联动
comments: false
---

[完整案例下载地址](http://download.csdn.net/detail/kesixin/9910075)


##HTML##

```
<html>
	<head>
		<title>JS全国城市三级联动</title>

		<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
		<meta http-equiv="description" content="this is my page">
		<meta http-equiv="content-type" content="text/html; charset=UTF-8">
		<script src="js/jquery-1.8.0.min.js" type="text/javascript"></script>
	</head>

	<body>
		<div align="center">
			</br>
			</br>
			</br>
			</br>
			</br>
			
			<form class="form-horizontal">
				<select  style="width:100px;height:30px;" id="s_province" name="s_province"></select>  
				<select  style="width:100px;height:30px;" id="s_city" name="s_city" ></select>  
				<select  style="width:100px;height:30px;" id="s_county" name="s_county"></select>  
			</form>
			
			<script class="resources library" src="js/area.js" type="text/javascript"></script>			
			<script type="text/javascript">_init_area();</script>
			
	</body>

</html>
```

##JS##

```

function Dsy(){
	this.Items = {};
}
Dsy.prototype.add = function(id,iArray){
	this.Items[id] = iArray;
}
Dsy.prototype.Exists = function(id){
	if(typeof(this.Items[id]) == "undefined") return false;
	return true;
}

function change(v){
	var str="0";
	for(i=0;i<v;i++){
		str+=("_"+(document.getElementById(s[i]).selectedIndex-1));
	};
	var ss=document.getElementById(s[v]);
	with(ss){
		length = 0;
		options[0]=new Option(opt0[v],opt0[v]);
		if(v && document.getElementById(s[v-1]).selectedIndex>0 || !v){
			if(dsy.Exists(str)){
				ar = dsy.Items[str];
				for(i=0;i<ar.length;i++){
					options[length]=new Option(ar[i],ar[i]);
				}//end for
				if(v){ options[0].selected = true; }
			}
		}//end if v
		if(++v<s.length){change(v);}
	}//End with
}

var dsy = new Dsy();

dsy.add("0",["北京市","天津市","上海市","重庆市","河北省","山西省","内蒙古","辽宁省","吉林省","黑龙江省","江苏省","浙江省","安徽省","福建省","江西省","山东省","河南省","湖北省","湖南省","广东省","广西","海南省","四川省","贵州省","云南省","西藏","陕西省","甘肃省","青海省","宁夏","新疆","香港","澳门","台湾省"]);
dsy.add("0_0_0",["东城区","西城区","崇文区","宣武区","朝阳区","丰台区","石景山区","海淀区","门头沟区","房山区","通州区","顺义区","昌平区","大兴区","怀柔区","平谷区","密云县","延庆县","延庆镇"]);
dsy.add("0_0",["北京市"]);
dsy.add("0_1_0",["和平区","河东区","河西区","南开区","河北区","红桥区","塘沽区","汉沽区","大港区","东丽区","西青区","津南区","北辰区","武清区","宝坻区","蓟县","宁河县","芦台镇","静海县","静海镇"]);
dsy.add("0_1",["天津市"]);
dsy.add("0_2_0",["黄浦区","卢湾区","徐汇区","长宁区","静安区","普陀区","闸北区","虹口区","杨浦区","闵行区","宝山区","嘉定区","浦东新区","金山区","松江区","青浦区","南汇区","奉贤区","崇明县","城桥镇"]);
dsy.add("0_2",["上海市"]);
dsy.add("0_3_0",["渝中区","大渡口区","江北区","沙坪坝区","九龙坡区","南岸区","北碚区","万盛区","双桥区","渝北区","巴南区","万州区","涪陵区","黔江区","长寿区","合川市","永川区市","江津市","南川市","綦江县","潼南县","铜梁县","大足县","荣昌县","璧山县","垫江县","武隆县","丰都县","城口县","梁平县","开县","巫溪县","巫山县","奉节县","云阳县","忠县","石柱土家族自治县","彭水苗族土家族自治县","酉阳土家族苗族自治县","秀山土家族苗族自治县"]);
dsy.add("0_3",["重庆市"]);

......

dsy.add("0",["北京市","天津市","上海市","重庆市","河北省","山西省","内蒙古","辽宁省","吉林省","黑龙江省","江苏省","浙江省","安徽省","福建省","江西省","山东省","河南省","湖北省","湖南省","广东省","广西","海南省","四川省","贵州省","云南省","西藏","陕西省","甘肃省","青海省","宁夏","新疆","香港","澳门","台湾省"]);

var s=["s_province","s_city","s_county"];//三个select的name
var opt0 = ["省份","地级市","区县"];//初始值
function _init_area(){  //初始化函数
	for(i=0;i<s.length-1;i++){
	  document.getElementById(s[i]).onchange=new Function("change("+(i+1)+")");
	}
	change(0);
}

```

![](http://upload-images.jianshu.io/upload_images/6673460-313e04aee6a02ff3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)