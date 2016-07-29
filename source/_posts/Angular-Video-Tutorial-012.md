---
title: 功能强大的Behavior
date: 2016-05-21 17:30:23
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/12.Useful%20Behavior.mp4" controls="true" width="100%"></video>

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
	<title>Useful Behavior</title>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="app.js"></script>
	<style>
		.enter{
			color: #f00;
		}
	</style>
</head>
<body>
	<div ng-app="myApp">
		<div leave enter="enter">Hello World</div>
	</div>
</body>
</html>
```

### app.js的代码

```
angular.module("myApp", [])

	.directive("enter", function(){
		return function(scope, element, attrs){
			element.bind("mouseenter", function(){
				element.addClass(attrs.enter);
			})
		}
	})

	.directive("leave", function(){
		return function(scope, element, attrs){
			element.bind("mouseleave", function(){
				element.removeClass(attrs.enter);
			})
		}
	})
```