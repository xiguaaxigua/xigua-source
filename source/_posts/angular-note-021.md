---
title: 021-校验用户输入-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-14 14:12:03
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要
本文讲述Angular中的表单验证。

## 正文
Angular自动为form元素增加了一些很好用的特性，使其更适合开发单页面应用。其中一个特性是，Angular允许你为表单的输入元素定义一个合法的状态，并且只有当所有元素都是合法状态时才允许提交表单。

例如，创建一个新用户注册表单，我们要求必须输入用户名和邮件地址，但是年龄字段是可选的，在这些字段被提交到服务器之前我们可以校验这几个输入项。把下面的例子加载到浏览器中，然后效果如下：

![](http://7xtoaz.com1.z0.glb.clouddn.com/xigua_angular_009)

我们想要确保用户在两个名称字段中都输入了值，并且输入了一个格式合法的邮件地址。另外如果他输入了年龄，那也是合法的。

利用Angular的form扩展以及大量的输入元素，我们可以在模板中实现以上效果，示例如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<script src="../angular.js"></script>
</head>
<body ng-app="myApp" ng-controller="HomeController">
	<h1>注册</h1>
	<form name="addUserForm" action="">
		<div>姓：<input type="text" ng-model="user.first" required></div>
		<div>名：<input type="text" ng-model="user.last" required></div>
		<div>邮箱：<input type="email" ng-model="user.email" required></div>
		<div>年龄：
			<input type="number" ng-model="user.age" ng-maxlength="1" ng-minlength="1">
		</div>
		<div><button>Submit</button></div>
	</form>
</body>
</html>
```

注意，我们使用了HTML5的required属性以及email和number两种输入类型，用来校验其中的一些字段。这一机制在Angular中工作的很好，而对于那些老式的不支持HTML5的浏览器来说，Angular将会自动使用指令来适配，从而实现相同的功能。

然后就可以添加一个控制器来处理提交功能了，只要修改form来引用控制器即可。

```
<form action="" name="addUserForm" ng-controller="AddUserController"></form>
```

在控制器中，我们可以通过$valid属性获取表单的校验状态。当表单中的所有输入项都合法时，Angular将会把这个属性设置为true。我们可以使用$valid属性来做很炫的事情，例如当表单没有输入完成时可以禁用Submit按钮。

我们可以在Submit按钮上添加一个ng-disabled指令，从而避免提交非法状态的表单：

```
<button ng-disabled="!addUserForm.$valid">Submit</button>
```

最后，我们想让控制器告诉用户，他已经注册成功了。控制器代码如下：

```
var app = angular.module('myApp', []);

app.controller('AddUserController', function($scope){
	$scope.message = '';

	$scope.addUser = function(){

		//由读者自己实现：把user真正保存到数据库中...
		$scope.message = 'Thanks, '+ $scope.user.first + ', we added you!';
	}
});
```