---
title: 记录一个遇到的关于浮动的BUG
date: 2016-07-02 16:03:44
tags: [HTML, CSS]
---
由于高度不够，浮动被卡。
<!--more-->

### 遇到的问题
期待的效果如下图：
![](http://7xtoaz.com1.z0.glb.clouddn.com/float0.jpg)

实际情况如下图：
![](http://7xtoaz.com1.z0.glb.clouddn.com/float2.jpg)

### 问题分析

显然，由于li的高度是由内容确定的，由于“新浪微博”这个li的内容相比其他li来说少，因此高度比其他的li低，造成该li之后的li卡在右侧，类似于当前浮动为right的假象。

解决办法：设置一个最小高度，例如`min-height:40px;`

解决后的效果：
![](http://7xtoaz.com1.z0.glb.clouddn.com/float3.jpg)


[点击访问Demo页面](http://7xtoaz.com1.z0.glb.clouddn.com/floatDemo.html)

附上Demo代码：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>一个浮动的bug</title>
	<style>
		*{
			margin: 0;
			padding: 0;
		}
		.clearfix:after{
			content:".";
			display:block;
			height:0;
			clear:both;
			visibility:hidden
		}
		.container{
			width: 1000px;
			margin: 0 auto;
		}
		.box{
			width: 100%;
		}
		h2{
			margin-top: 100px;
		}
		.box li{
			list-style-type: none;
			float: left;
			width: 240px;
			margin: 5px;
		}
		.box1 li{
			background: #FF5454;
		}
		.box2 li{
			background: #00bcd4;
		}
		.box3 li{
			background: #9c27b0;
			min-height: 40px;
		}
	</style>
</head>
<body>
	<div class="container">
		<h2>期待的效果：</h2>
		<div class="box box1">
			<ul class="clearfix">
				<li>
					<h3>微信</h3>
					<p>腾讯的产品</p>
				</li>
				<li>
					<h3>支付宝</h3>
					<p>阿里的产品</p>
				</li>
				<li>
					<h3>QQ</h3>
					<p>腾讯的产品</p>
				</li>
				<li>
					<h3>新浪微博</h3>
					<p>网易的产品</p>
				</li>
				<li>
					<h3>iPhone</h3>
					<p>Apple的产品</p>
				</li>
			</ul>
		</div>

		<h2>遇到的问题：</h2>
		<div class="box box2">
			<ul class="clearfix">
				<li>
					<h3>微信</h3>
					<p>腾讯的产品</p>
				</li>
				<li>
					<h3>支付宝</h3>
					<p>阿里的产品</p>
				</li>
				<li>
					<h3>QQ</h3>
					<p>腾讯的产品</p>
				</li>
				<li>
					<h3>新浪微博</h3>
				</li>
				<li>
					<h3>iPhone</h3>
					<p>Apple的产品</p>
				</li>
			</ul>
		</div>
	
		<h2>解决后的效果：</h2>
		<div class="box box3">
			<ul class="clearfix">
				<li>
					<h3>微信</h3>
					<p>腾讯的产品</p>
				</li>
				<li>
					<h3>支付宝</h3>
					<p>阿里的产品</p>
				</li>
				<li>
					<h3>QQ</h3>
					<p>腾讯的产品</p>
				</li>
				<li>
					<h3>新浪微博</h3>
				</li>
				<li>
					<h3>iPhone</h3>
					<p>Apple的产品</p>
				</li>
			</ul>
		</div>
	</div>
</body>
</html>
```