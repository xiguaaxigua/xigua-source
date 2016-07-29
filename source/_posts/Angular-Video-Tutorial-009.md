---
title: 第一个指令
date: 2016-05-20 11:00:12
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/8.Built%20in%20Filter.mp4" controls="true" width="100%"></video>

## 笔记

### 目录结构：
demo/
——app.js
——index.html

### index.html的代码：

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>First Directive</title>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="app.js"></script>
</head>
<body>
	<div ng-app="superhero">
		<superman></superman>
	</div>
</body>
</html>
```

### app.js的代码

```
var app = angular.module('superhero', []);

app.directive("superman", function(){

	return {
		restrict: "E",
		template: "<div>Here I am to save the day</div>"	
	}
});
```

### 心得

<b>1.指令的作用：实现语义化标签</b>

我们常用的HTML标签是这样的：

```
<div>
	<span>内容</span>
</div>
```

而使用Angular的directive指令可以实现以下效果的标签：

```
<div>
	<superman>内容</superman>
</div>
```

很类似于JSP或者Struts框架里的tablib。

<b>2.举个栗子！</b>

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>First Demo</title>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
</head>
<body>
	<div ng-app="helloworld">
		<hello></hello>
	</div>
	<script>
		var app = angular.module('helloworld', []);

		app.directive("hello", function(){

			return {
				restrict: "E",
				template: "<div>Hello World!</div>"	,
				replace: true
			}
		});
	</script>
</body>
</html>
```

下面介绍restrict配置：
![](http://7xtoaz.com1.z0.glb.clouddn.com/xigua_angular_010)

关于replace：
+ 如果值为true，将会替换hello，也就是说页面中不会出现```<hello></hello>```。
+ 如果值为false，将不会替换hello。
+ 默认值为false。

可见：<b>指令的作用是把我们自定义的语义化标签替换成浏览器能够认识的HTML标签。</b>

### 相关文章

+ [《AngularJS》5个实例详解Directive（指令）机制](http://damoqiongqiu.iteye.com/blog/1917971)
