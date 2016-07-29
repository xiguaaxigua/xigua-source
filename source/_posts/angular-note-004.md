---
title: 004-实例：购物车-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-07 12:50:23
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要
+ 简单购物车实例
+ Angular常见指令
	+ ng-app（声明Angular的控制区域）
	+ ng-model（数据）
	+ ng-controller（控制器）
	+ filter（修正器）
	+ ng-repeat（循环）
	+ ng-click（Angular里的click事件）

## 正文
我们来看一个稍微大一点的例子，它将会展示Angular的更多特性。想象一下，我们打算内构建一款购物应用，需要在应用中的某个地方展示用户的购物车，并且用户能够对其进行编辑。我们直接跳到购物车这个部分：
``` html
<html>
	<head>
		<meta charset="UTF-8">
		<title>Your Shopping Cart</title>
	</head>
	<body ng-app="myApp" ng-controller="CartController">
		<h1>Your Order</h1>
		<div ng-repeat="item in items">
			<span>{{item.title}}</span>
			<input ng-model="item.quantity">
			<span>{{item.price | currency}}</span>
			<span>{{item.price * item.quantity | currency}}</span>
			<button ng-click="remove($index)">Remove</button>
		</div>
		<script src="../angular.js"></script>
		<script>
			var app = angular.module('myApp', []);
			app.controller('CartController', function CartController($scope) {
				$scope.items = [
					{title: '商品1', quantity: 8, price: 4},
					{title: '商品2', quantity: 7, price: 3},
					{title: '商品3', quantity: 6, price: 2}
				];

				$scope.remove = function(index){
					$scope.items.splice(index, 1);
				}
			});
		</script>
	</body>
</html>
```
最终UI截屏如图所示：
![](http://7xtoaz.com2.z0.glb.clouddn.com/xigua_angular_005)

下面是对这段代码的一个概要解释，本书后续的内容将会对此做更深入的解析。

我们从头开始：

``` html
<html ng-app>
```
ng-app属性将用来告诉Angular页面中的哪一部分需要接受它的管理。既然我们把这个属性放在html标签上，那么就是在告诉Angular，我们希望它管理整个页面。通常情况下你只需要这么做就可以了，但是，如果你打算把Angular集成到一个现有的应用中，而这个应用使用了其他的方式来管理页面，那么你可能需要把ng-app属性放到应用中的一个div上。

``` html
<body ng-controller="CartController">
	
</body>
```
在Angular中，你将会使用一种叫做控制器的JavaScript类来管理页面中的区域。在body标签中引入一个控制器，就是在声明CartController将会管理介于body和/body之间的所有内容。

``` html
<div ng-repeat="item in items"></div>
```
ng-repeat的意思是，对于items数组中的每一个元素，都把div中的DOM结构复制一份（<b>包括div本身</b>）。对于div的每一份拷贝，都会把一个叫做item的属性设置给它，这样我们就可以在模板中使用这份拷贝的元素了。如你所见，这样一来就会产生3个div，其中分别包含了产品的名称、数量、单价、总价，以及一个可以用来完全删除一个项目的按钮。

``` html
<span>{{item.title}}</span>
```
正如我们在“Hello World”那个例子中所展示的，通过

```
{{}}
```

进行数据绑定让我们可以把变量的值插入到页面的一部分中，同时能够保证它会自动同步。完整的表达式

```
{{item.title}}
```

将会获取到循环中的当前title，然后把这个item的title属性值插入到DOM中。

``` html
<input ng-model="item.quantity">
```
定义ng-model将会在输入框和itme.quantity的值之间创建数据绑定的关系。

span里面的

```
{{}}
```

将会创建一个单向的关系，也就是说“在这里插入一个值”。这正是我们所要达到的效果，但是应用还需要知道用户何时修改了数量，这样它就可以修改总价了。

使用ng-model我们就可以保持变更与模型同步了。ng-model声明将会把item.quantity的值插入到文本框中，同时，不管什么时候，只要用户输入了新的数值，它就会自动更新item.quantity的值。
``` html
<span>{{item.price | currency}}</span>
<span>{{item.price * item.quantity | currency}}</span>
```

我们想让单价和总价能够以美元的格式显示。Angular带有一种叫做过滤器（filter）的特性，我们可以用它来转换文本的格式，有一个内置过滤器叫做currency（货币），它可以为我们实现美元格式化。在下一章中我们将会看到关于过滤器的更多内容。

```
<button ng-click="remove($index)">Remove</button>
```

这个按钮可以让用户从他们的购物车中删除项目，点击产品旁边的Remove按钮即可，因为我们已经设置好了，点击这个按钮将会调用remove()函数。同时我们还会把$index传递过去，$index包含了ng-repeat过程中的循环计数，这样以来我们就知道要删除哪一个项目了。

``` javascript
function CartController($scope){
	
}
```
CartController将会负责管理购物车的业务逻辑。在函数的形参中放一个$scope就可以告诉Angular：控制器需要一个叫做$scope的东西。我们可以通过$scope把数据绑定到UI中的元素上。

``` javascript
$scope.items = [
	{title: '商品1', quantity: 8, price: 4},
	{title: '商品2', quantity: 7, price: 3},
	{title: '商品3', quantity: 6, price: 2}
];
```

通过定义$scope.items，我们创建了一个虚拟的hash型数据，用来表示用户购物车中的项目集合。我们想让这些项目能够对UI的数据绑定有效，所以我们要把它们设置到$scope上。

当然，这个例子的真实版本不可能只是在内存里面运行，他还需要与服务端进行交互并正确的把数据持久化。我们将会在后续的章节中讨论这个话题。

``` javascript
$scope.remove = function(index){
	$scope.items.splice(index, 1);
}
```
在绑定到UI的时候，我们希望remove()函数也有效，所以我们也需要把它设置到$scope上。对于购物车的纯内存版，remove()函数可以只从数组中删除元素。由于ng-repeat所创建的div列表都是绑定在数据上的，所以当数组中的项目消失时，这个列表将会自动收缩。记住，无论何时，只要用户点了Remove按钮，就会从UI中调用remove()函数。


