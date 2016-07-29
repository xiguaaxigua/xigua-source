---
title: Angular中的ui-router
date: 2016-05-19 14:48:22
tags: [Angular]
---
Angular的学习笔记
<!--more-->

### 什么是ui-router？
ui-router是一个web客户端的解决方案，它最大的作用是将web界面分块。

更多内容请移步[官网](https://angular-ui.github.io/ui-router/site/#/api/ui.router)。

最初的web访问模型，是这样的：
![](http://img.blog.csdn.net/20150310095845292)

我们访问page1，然后访问page2.... 

在新的模型中它变成了这个样子：  

![](http://img.blog.csdn.net/20150310095756886)

访问效果是一样的，但是从设计上已经有了变化。它变成了：只有一张网页，在网页中有不同的区域，每个区域都可擦写。

### 代码实现：
1.下载js文件，例如：

```
<script src="//cdn.bootcss.com/angular-ui-router/1.0.0-alpha0/angular-ui-router.min.js"></script>
```

2.在模板中添加注入位置，例如：

```
<div ui-view></div>
```

或者：

```
<ui-view></ui-view>
```

3.在模板中，添加触发器，例如：

```
<div ui-sref="first"></div>
```

4.在控制器中，配置路由参数，例如：

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
```

### 完整案例

```
<!doctype html>
<html ng-app="app">
	<head>
	<script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
	<script src="//cdn.bootcss.com/angular-ui-router/1.0.0-alpha0/angular-ui-router.min.js"></script>
	<script>
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
	</script>
	</head>
<body>
	<ui-view></ui-view>
</body>
</html>
```

templates下的first.html的代码：

```
<div ui-sref="second">
	Go to Second
</div>
```

templates下的second.html的代码：

```
Hello Ui-Router!
```

### 相关文章
+ [《AngularJS ui-router (嵌套路由)》](http://www.oschina.net/translate/angularjs-ui-router-nested-routes?print)
+ [《angular 使用 ui-router 设计网页》](http://blog.csdn.net/fansongy/article/details/44162685)
+ [《Angular-UI-Router 学习笔记》](http://www.tuicool.com/articles/zeiy6ff)