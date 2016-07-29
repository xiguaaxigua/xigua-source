---
title: 了解Angular中的scope
date: 2016-05-21 22:32:21
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
	<title>Understanding Isolate Scope</title>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="app.js"></script>
</head>
<body>
	<div ng-app="choreApp">
		<div ng-controller="ChoreCtrl">
			<kid done="logChore(chore)"></kid>			
		</div>
	</div>
</body>
</html>
```

### app.js的代码

```
angular.module('choreApp', [])
	
	.controller("ChoreCtrl", function($scope){
		$scope.logChore = function(chore){
			alert(chore + " is Done!");
		}
	})

	.directive('kid', function(){
		return {
			restrict: "E",
			scope: {
				done: "&"
			},
			template: '<input type="text" ng-model="chore"/><br/>' + '{{chore}}<br/>' + '<div class="button" ng-click="done({chore: chore})">I\`m done! </div>'
		}
	})
```

### 关于scope作用域
+ 1.AngularJS中，子作用域一般都会通过JavaScript原型继承机制继承其父作用域的属性和方法。但有一个例外：在directive中使用scope: { ... }，这种方式创建的作用域是一个独立的"Isolate"作用域，它也有父作用域，但父作用域不在其原型链上，不会对父作用域进行原型继承。这种方式定义作用域通常用于构造可复用的directive组件.

+ 2.如果我们在子作用域中访问一个父作用域中定义的属性，JavaScript首先在子作用域中寻找该属性，没找到再从原型链上的父作用域中寻找，如果还没找到会再往上一级原型链的父作用域寻找。在AngularJS中，作用域原型链的顶端是$rootScope，JavaScript寻找到$rootScope为止.

+ 3.scope: { ... } - directive创建一个独立的“Isolate”作用域，没有原型继承。这是创建可复用directive组件的最佳选择。因为它不会直接访问/修改父作用域的属性，不会产生意外的副作用。

### Isolate scope 引用修饰符
+ 1、 = or =attr “Isolate”作用域的属性与父作用域的属性进行双向绑定，任何一方的修改均影响到对方，这是最常用的方式；

+ 2、 @ or @attr “Isolate”作用域的属性与父作用域的属性进行单向绑定，即“Isolate”作用域只能读取父作用域的值，并且该值永远的String类型；

+ 3、 & or &attr “Isolate”作用域把父作用域的属性包装成一个函数，从而以函数的方式读写父作用域的属性，包装方法是$parse

### directive 与 controller 数据传递和通信
+ 1、父controller监听全局scope(父scope)变量, 并广播事件给子scope(directive scope,每个directvie都有自己独立的scope作用域)
     
+ 2、directive 定义本地scope,通过=、@、&(方法)字符显示引用全局scope
     
+ 3、directive scope(子scope)通过parent[$scope.$parent.xxx]引用全局scope的属性
     
+ 4、directive监听全局scope变量变化,可以通过$scope.$parent.$watch方法