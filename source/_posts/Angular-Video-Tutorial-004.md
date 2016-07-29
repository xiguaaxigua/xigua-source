---
title: 在控制器间共享数据
date: 2016-05-19 14:59:30
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/4.Sharing%20Data%20Between%20Controllers.mp4" controls="true" width="100%"></video>

## 笔记

### 目录结构：
demo/
——templates/
————first.html
————second.html
——app.js
——index.html

### index.html的代码：

```
<!DOCTYPE html>
<html lang="en" ng-app="app">
<head>
	<meta charset="UTF-8">
	<title>Sharing Data Between Controllers</title>
	<link rel="stylesheet" href="http://cdn.bootcss.com/bootstrap/3.2.0/css/bootstrap.min.css">
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="//cdn.bootcss.com/angular-ui-router/1.0.0-alpha0/angular-ui-router.min.js"></script>
	<script src="app.js"></script>
	<style>
		body{
			padding: 10px;
		}
	</style>
</head>
<body>
	<ui-view></ui-view>
</body>
</html>
```

### app.js的代码

```
angular.module("app", ["ui.router"])
	
	.config(function config($stateProvider){

		$stateProvider.state("index", {
			url: "",
			controller: "FirstCtrl as first",
			templateUrl: "templates/first.html"
		})

		$stateProvider.state("second", {
			url: "/second",
			controller: "SecondCtrl as second",
			templateUrl: "templates/second.html"
		})

	})

	.service("greeting", function Greeting(){

		var greeting = this;
		greeting.message = "Default";
	})

	.controller("FirstCtrl", function FirstCtrl(greeting){

		var first = this;
		first.greeting = greeting;
	})
	
	.controller("SecondCtrl", function SecondCtrl(greeting){

		var second = this;
		second.greeting = greeting;
	})
```

### first.html中的代码

```
<input type="text" ng-model="first.greeting.message">
<div ng-class="first.greeting.message">
	{{ first.greeting.message }} {{ "World" }}
</div>

<div ui-sref="second">
	Go to Second
</div>
```

### second.html中的代码

```
<h1>{{ second.greeting.message }}</h1>
```

TIPS：
+ 需要引入bootstrap.css和ui-router.js
+ 关于ui-router，请移步[Angular中的ui-router](http://xiguaaxigua.cn/2016/05/19/ui-router-angular/)