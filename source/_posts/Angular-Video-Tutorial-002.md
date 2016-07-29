---
title: Angular中的数据绑定
date: 2016-05-19 13:41:54
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/2.Basic%20Binding%20in%20Angular.mp4" controls="true" width="100%"></video>

## 笔记

代码如下：

```
<!DOCTYPE html>
<html lang="en" ng-app>
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<link rel="stylesheet" href="http://cdn.bootcss.com/bootstrap/3.2.0/css/bootstrap.min.css">
	<style>
		body{
			padding: 10px;
		}
	</style>
</head>
<body>
	<input type="text" ng-model="first.greeting">
	<br>
	<div ng-class="first.greeting">
		{{ first.greeting }} {{ "World" }}
	</div>
</body>
<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
</html>
```

TIPS：
+ 视频中介绍的输入“alert alert-danger”出现的效果，需要引入Bootstrap.css