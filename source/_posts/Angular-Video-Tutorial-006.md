---
title: Angular中的过滤器
date: 2016-05-19 16:40:58
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/6.Filter.mp4" controls="true" width="100%"></video>

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
	<title>Filtering in Angular</title>
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
		<h1>{{data.message | reverse}}</h1>
	</div>
</body>
</html>
```

### app.js的代码

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

myApp.filter('reverse', function(){
    return function(text){
        return text.split("").reverse().join("");
    }
});

myApp.controller('SecondCtrl',
    function SecondCtrl($scope, Data) {
        $scope.data = Data;

        $scope.reversedMessage = function() {
            return $scope.data.message.split("").reverse().join("");
        }
    }
);

```