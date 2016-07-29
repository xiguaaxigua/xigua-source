---
title: Angular中的指令限制
date: 2016-05-21 18:00:40
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/13.Directive%20Restrictions.mp4" controls="true" width="100%"></video>

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
	<title>Directive Restrictions</title>
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

// app.directive("superman", function(){

// 	return {
// 		restrict: "A",
// 		link: function(){
// 			alert('I`m Working!');
// 		}	
// 	}
// });

// app.directive("superman", function(){

// 	return {
// 		restrict: "C",
// 		link: function(){
// 			alert('I`m Working!');
// 		}	
// 	}
// });

app.directive("superman", function(){

	return {
		restrict: "M",
		link: function(){
			alert('I`m Working!');
		}	
	}
});
```

TIPS:
+ 视频中的例子，分别介绍了restrict的值为A、C、M的情况，此外[之前的例子](http://xiguaaxigua.cn/2016/05/20/Angular-Video-Tutorial-009/)中介绍过值为E代表着element。