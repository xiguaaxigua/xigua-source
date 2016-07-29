---
title: Angular中的控制器
date: 2016-05-19 14:55:30
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/3.Controllers.mp4" controls="true" width="100%"></video>

## 笔记

### 目录结构：
demo/
——app.js
——index.html

### index.html的代码：

```
<!DOCTYPE html>
<html ng-app="app">
<head>
	<meta charset="UTF-8">
	<title>Controllers in Angular</title>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="app.js"></script>
</head>
<body>
	<div ng-controller="FirstCtrl">
		<h1>{{data.message + "World"}}</h1>

		<div class="{{data.message}}">
			Wrap me with foundation component.
		</div>
	</div>
</body>
</html>
```

### app.js的代码：

```
angular.module("app", [])
	
	.controller("FirstCtrl", function ($scope){
		$scope.data = {message: "Hello "};
	})
```