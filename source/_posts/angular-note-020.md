---
title: 020-使用指令修改DOM-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-14 13:42:01
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 正文
指令扩展了HTML语法，同时它也是使用自定义的元素和属性把行为和DOM转换关联到一起的方式。通过这些指令，你可以创建可复用的UI组件，配置你的应用，并且可以做到你能想象到的几乎所有事情，这些事情都可以在你的UI模板中实现。

你可以使用Angular内置的指令来编写应用，但是也会遇到需要自己编写指令的情况。当你想要以某种方式处理浏览器事件或者修改DOM，而这些东西有没有内置指令可以支持时，你就会知道是时候深入理解指令体系了。你编写的代码将会位于你所编写的指令中，而不是控制器、服务或应用的其他地方。

与服务一样，你可以通过模块对象的API来定义指令，只要调用模块实例的directive()函数即可，其中directiveFunction是一个工厂函数，用来定义指令的特性。

```
var appModule = angular.module('appModule', [...]);
appModule.directive('directiveName', directiveFunction);
```

编写指令工厂函数是一个非常深的领域，我们在本书中专门开辟了一整章的内容来讨论这一话题。我了充分调用你的兴趣，我们先来看一个简单的例子。

HTML5中带有一个很棒的新属性，叫做autofocus，它可以把键盘的焦点定位到一个输入元素中。通过它，用户能够立即通过键盘和元素进行交互，而不必先点击元素。这一点很棒，因为他让你可以通过声明的方式来描述需要浏览器做什么，而没有必要编写任何JavaScript代码。但是，如果i想把焦点放到一些非交互性元素上，例如超链接或者div，应该怎么做呢？如果你想让这种机制在不支持HTML5的浏览器中也能工作，又应该怎么做呢？我们可以通过一个指令来实现。

```
var appModule = angular.module('app', []);
appModule.directive('ngbkFocus', function(){
	
	return {
		link: function(scope, element, attrs, controller){
			element[0].focus();
		}
	}
});
```

在上面的代码中，我们返回了指令的配置对象，其中带有特定的link函数。link函数会获取外层scope的引用、它所存在的DOM元素、传递给指令的一个属性数组，以及DOM元素上的控制器——如果有控制器存在的话。这里，我们只需要获取元素，然后调用它的focus()方法即可。

然后我们就可以把它用到例子中，如下：

### index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<script src="../angular.js"></script>
</head>
<body ng-app="myApp" ng-controller="SomeController">
	<button ng-click="clickUnFocused()">
		Not Focused
	</button>
	<button ng-click="clickFocused()">
		I`m very Focused!
	</button>
	<div>{{message.text}}</div>
</body>
</html>
```

### controller.js

```
var app = angular.module('myApp', []);

app.controller('SomeController', function($scope){
	$scope.message = {text: 'Nothing clicked yes'};

	$scope.clickUnFocused = function(){
		$scope.message.text = 'Unfocused button clicked';
	}

	$scope.clickFocused = function(){
		$scope.message.text = 'Focused button clicked';
	}
});
```

当页面加载完成之后，用户将会看到一个按钮，其标签为“I`m very Focused!”，他获得了高亮焦点。按下空格或者回车键江湖导致一次点击动作，并且会调用ng-click，它将会把div上的文本设置为“Focused button clicked”。在浏览器中打开这个例子，我们将会看到如下图所示效果：

![](http://7xtoaz.com1.z0.glb.clouddn.com/xigua_angular_008)

完整代码如下：
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<script src="../angular.js"></script>
</head>
<body ng-app="myApp" ng-controller="SomeController">
	<button ng-click="clickUnFocused()">
		Not Focused
	</button>
	<button ng-click="clickFocused()">
		I`m very Focused!
	</button>
	<div>{{message.text}}</div>

	<script>
		var app = angular.module('myApp', []);

		app.controller('SomeController', function($scope){
			$scope.message = {text: 'Nothing clicked yes'};

			$scope.clickUnFocused = function(){
				$scope.message.text = 'Unfocused button clicked';
			}

			$scope.clickFocused = function(){
				$scope.message.text = 'Focused button clicked';
			}
		});
	</script>
</body>
</html>
```
