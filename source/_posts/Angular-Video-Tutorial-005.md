---
title: 在Scope上定义一个方法
date: 2016-05-19 16:30:53
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->
## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/5.Defining%20a%20Method%20on%20The%20Scope.mp4" controls="true" width="100%"></video>

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
	<title>Controllers in Angular</title>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="app.js"></script>
</head>
<body>
	<div ng-controller="FirstCtrl">
		<input type="text" ng-model="data.message">
		<h1>{{data.message}}</h1>
	</div>
	<div ng-controller="SecondCtrl">
		<input type="text" ng-model="data.message">
		<h1>{{reversedMessage()}}</h1>
	</div>
</body>
</html>
```

### app.js的代码：

```
var myApp = angular.module('myApp', []);
myApp.factory('Data', function() {
    return { message: "I`m data from a service" };
})

myApp.controller('FirstCtrl', 
	function FirstCtrl($scope, Data) {
    	$scope.data = Data;
	}
);

myApp.controller('SecondCtrl',
    function SecondCtrl($scope, Data) {
        $scope.data = Data;

        $scope.reversedMessage = function() {
            return $scope.data.message.split("").reverse().join("");
        }
    }
);

```
