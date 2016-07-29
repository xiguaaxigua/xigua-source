---
title: 017-使用过滤器格式化数据-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-13 11:55:08
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要
+ 数据的格式化（过滤器）与处理数据的逻辑是无关的，过滤器应该存放在模板中，这点类似于Smarty。
+ 本文讲述Angular中Filter的使用。

## 正文
你可以使用过滤器来声明应该如何变换数据格式，然后再显示给用户，你只要在模板中使用一个插值变量即可。使用过滤器的语法是：

```
{{expression | filterName : parameter1 : ... parameterN}}
```

这里的表达式可以是任意的Angular表达式，filterName是你需要使用的过滤器名称，过滤器的多个参数之间使用冒号分隔。这些参数自身也可以是任何合法的Angular表达式。

Angular内置了很多过滤器，例如我们在前面见过的currency：

```
{{12.9 | currency}}
```

以上代码将会显示如下结果：

$12.9

我们把插值变量声明在视图里面（而不是在控制器或者模型里面），里面数字前面的美元符号对人类来说很重要，而对我们用来处理数值的逻辑来说是没有意义的。

Angular内置的其他过滤器还有date、number、uppercase等。

在绑定的过程中，可以用管道符号把过滤器连接起来。例如，对于上面这个例子，我们可以加一个number过滤器来把小数点后边的数值格式化掉，number过滤器会把数值作为参数传递给round函数。所以：

```
{{12.9 | currency | number:0}}
```

将会显示成：

$13

你不必受限于内置的过滤器，自己编写过滤器也非常简单。例如，如果我们需要为标题文字创建首字母大写的字符串，可以像下面这样做：

```
var homeModule = angular.module('HomeModule', []);

homeModule.filter('titleCase', function(){
	var titleCaseFilter = function(input){
		var words = input.split(' ');

		for(var i=0; i<words.length; i++){
			words[i] = words[i].charAt(0).toUpperCase() + words[i].slice(1);
		}

		return words.join(' ');
	}

	return titleCaseFilter;
})

```

对应的模板如下：

```
<body ng-app="HomeModule" ng-controller="HomeController">
	<h1>{{pageHeading | titleCase}}</h1>
</body>
```

然后通过控制器把pageHeading作为一个模型变量插入进去：

```
function HomeController($scope){
	$scope.pageHeading = 'behold the majesty of your page title';
}
```

我们将会看到如下图所示效果：

![](http://7xtoaz.com1.z0.glb.clouddn.com/xigua_angular_007)

完整代码如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<script src="../angular.js"></script>
</head>
<body ng-app="myApp" ng-controller="HomeController">
	<h1>{{pageHeading | titleCase}}</h1>

	<script>
		var app = angular.module('myApp', []);

		app.filter('titleCase', function(){
			var titleCaseFilter = function(input){
				var words = input.split(' ');

				for(var i=0; i<words.length; i++){
					words[i] = words[i].charAt(0).toUpperCase() + words[i].slice(1);
				}

				return words.join(' ');
			}

			return titleCaseFilter;
		});

		app.controller('HomeController', function($scope){
			$scope.pageHeading = 'behold the majesty of your page title';
		});
	</script>
</body>
</html>
```