---
title: Angular中的基本行为
date: 2016-05-21 16:35:20
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/11.Besic%20Behaviors.mp4" controls="true" width="100%"></video>

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
	<title>Basic Behavior</title>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="app.js"></script>
</head>
<body>
	<div ng-app="myApp">
		<div enter leave>Hello World</div>
	</div>
</body>
</html>
```

### app.js的代码

```
var app = angular.module("myApp", []);

app.directive("enter", function(){

	return function(scope, element){
		element.bind("mouseenter", function(){
			console.log('I`m entering you!');
		})
	}
})

app.directive("leave", function(){

	return function(scope, element){
		element.bind("mouseleave", function(){
			console.log("I`m leaving you!")
		})
	}
})
```

### app.js的代码也可以这样写

```
angular.module("myApp", [])

	.directive("enter", function(){
		return function(scope, element){
			element.bind("mouseenter", function(){
				console.log('I`m entering you!');
			})
		}
	})

	.directive("leave", function(){
		return function(scope, element){
			element.bind("mouseleave", function(){
				console.log('I`m leaving you!');
			})
		}
	})
```

博主在写这篇文章的时候遇见一个非常无语的bug，写本例的时候发现明明是按照套路来的，但是总是报错。博主就把之前一个运行正常的demo拷了过来，结果还！是！报！错！(・-・*)。懵逼的博主一怒之下删掉了本例的文件夹，重写了一份，结果竟！然！好！了！等我回头想追查一下是哪儿出错的时候，已经晚了，报错的那个文件夹已经删掉了。博主持续懵逼中...期待有小伙伴解惑(づ｡◕‿‿◕｡)づ
