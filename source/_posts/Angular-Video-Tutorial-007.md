---
title: Angular中的ng-repeat
date: 2016-05-20 10:10:53
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/7.ngRepeat%20and%20Filtering%20Data.mp4" controls="true" width="100%"></video>

## 笔记

### 目录结构：
demo/
——app.js
——index.html

### index.html的代码：

```
<!DOCTYPE html>
<html ng-app="myApp">
<head>
	<meta charset="UTF-8">
	<title>ngRepeat and Filtering Data</title>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="app.js"></script>
</head>
<body>
	<div ng-controller="AvengersCtrl">
		<input type="text" ng-model="search.$">
		<table>
			<tr ng-repeat="actor in avengers.cast | filter:search">
				<td>{{actor.name}}</td>
				<td>{{actor.character}}</td>
			</tr>
		</table>
	</div>
</body>
</html>
```

### app.js的代码

```
var myApp = angular.module('myApp', []);

myApp.factory('Avengers', function() {
	var Avengers = {};
	Avengers.cast = [{
		name: "老大",
		character: "1111"
	},
	{
		name: "老二",
		character: "2222"
	},
	{
		name: "老三",
		character: "3333"
	},
	{
		name: "老四",
		character: "4444"
	},
	{
		name: "老五",
		character: "5555"
	},
	{
		name: "老六",
		character: "6666"
	},
	{
		name: "老七",
		character: "7777"
	}];
    return Avengers;
});

myApp.controller("AvengersCtrl", function($scope, Avengers){
	$scope.avengers = Avengers;
});
```

TIPS：
1.下面这种情况搜索匹配name和character：

```
<input type="text" ng-model="searchText">
...
	<tr ng-repeat="actor in avengers.cast | filter:searchText">
...

```

或者：

```
<input type="text" ng-model="search.$">
...
	<tr ng-repeat="actor in avengers.cast | filter:search">
...
```

2.下面这种情况搜索匹配name，不匹配character：

```
<input type="text" ng-model="search.name">
...
	<tr ng-repeat="actor in avengers.cast | filter:search">
...
```

3.下面这种情况搜索匹配character，不匹配name：

```
<input type="text" ng-model="search.character">
...
	<tr ng-repeat="actor in avengers.cast | filter:search">
...
```