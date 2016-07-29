---
title: 编写你的第一个指令
date: 2016-05-21 10:47:48
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/10.Write%20Your%20First%20Directive.mp4" controls="true" width="100%"></video>

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
	<title>Write Your First Directive</title>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="app.js"></script>
</head>
<body ng-app="egghead" ng-controller="AppCtrl as app">
	<div my-first-directive message="Angular"></div>
</body>
</html>
```

### app.js的代码

```
var egghead = angular.module("egghead", []);

egghead.controller("AppCtrl", function(){
	var app = this;

	app.message = "Hello";
});

egghead.directive('myFirstDirective', function(){
	return function(scope, element, attrs){
		element.text(scope.app.message + " " + attrs.message);
	}
});
```

TIPS：
1.app.js中定义指令的方式是小驼峰，但是在模板中调用指令的时候要使用“-”间隔。在上例中，myFirstDirective对应着模板中的my-first-directive。

2.模板中，下面这两种方式是等价的。

```
<div my-first-directive></div>
```

```
<div my-first-directive=""></div>
```

3.在指令中传入element（当前元素）和attrs（属性），可以调用模板中存放指令位置的元素和当前元素的属性，类似于jQuery，看到这里不得不说，Angular好腻害♪(＾∀＾●)ﾉ。

4.在模板中，``` ng-controller="AppCtrl as app" ```是什么意思？

答案是：别名。将app作为AppCtrl控制器的别名。

在app.js中的AppCtrl中，可见：

```
var app = this;

app.message = "Hello";
```

那为什么不用这种方式呢？

```
egghead.controller("AppCtrl", function($scope){
	$scope.message = "Hello";
});
```

答案是我也不知道，此处留个坑吧。