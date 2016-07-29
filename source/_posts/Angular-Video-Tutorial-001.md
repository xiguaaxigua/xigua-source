---
title: 将Angular添加到HTML页面
date: 2016-05-19 12:59:30
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/1.First%20Step-%20Adding%20Angular%20to%20the%20HTML%20Page.mp4" controls="true" width="100%"></video>

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
+ 视频中的CDN需要翻墙，你懂得，上述代码中已经更换为国内CDN，以后都会这样
+ 请注意：ng-bind="hello"和{ {hello} }的区别