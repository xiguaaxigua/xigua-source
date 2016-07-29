---
title: 014-使用$watch监控数据模型的变化-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-12 11:23:05
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

### 使用$watch监控数据模型的变化

在$scope内置的所有函数中，用得最多的可能就是$watch函数了，当你的数据模型中某一部分发生变化时，$watch函数可以向你发出通知。你可以监控单个对象的属性，也可以监控需要经过计算的结果（函数），实际上只要能够被当作属性访问到，或者可以当作一个JavaScript函数被计算出来，就可以被$watch函数监控。它的函数签名为：

```
$watch(watchFn, watchAction, deepWatch)
```

其中每个参数的详细含义如下：

#### watchFn

该参数是一个带有Angular表达式或者函数的字符串，它会返回被监控的数据模型的当前值。这个表达式将会被执行很多次，所以你要保证他不会产生其他副作用。也就是说，要保证它可以被调用很多次而不会改变状态。基于同样的原因，监控表达式应该很容易被计算出来。如果你使用字符串传递了一个Angular表达式，那么它将会针对调用它的那个作用域中的对象而执行。

#### watchAction

这是一个函数或者表达式，当watchFn发生变化时会被调用。如果是函数的形式，它将会接收到watchFn的新旧两个值，以及作用域对象的引用。其函数签名为function(newValue, oldValue, scope)。

#### deepwatch

如果设置为true，这个可选的布尔型参数将会命令Angular去检查被监控对象的每个属性是否发生了变化。如果你想要监控数组中的元素，或者对象上的所有属性，而不只是监控一个简单的值，你就可以使用这个参数。由于Angular需要遍历数组或者对象，如果集合比较大，那么运算负担就会比较重。

$watch函数会返回一个函数，当你不再需要接受变更通知时，可以用这个返回的函数注销监控器。

如果我们需要监控一个属性，然后接着注销监控，我们可以使用一下代码：

```
...
var dereg = $scope.$watch('someModel.someProperty', callbackOnChange());
...
dereg();
```

我们再回到第一章的购物车案例，把它的功能扩充完整。例如，当用户添加到购物车中的商品价值超过100美元的时候，我们会给他10美元的折扣。我们将会使用下面这种模板：

```
<div ng-controller="CartController">
	<div ng-repeat="item in items">
		<span>{{item.title}}</span>
		<input ng-model="item.quantity">
		<span>{{item.price | currency}}</span>
		<span>{{item.price * item.quantity | currency}}</span>
	</div>
	<div>Total:{{totalCart() | currency}}</div>
	<div>Discount:{{bill.discount | currency}}</div>
	<div>Subtotal:{{subtotal() | currency}}</div>
</div>
```

而CartController看起来可能像下面这样：

```
function CartController($scope){
	$scope.bill = {};

	$scope.items = [
		{title: '商品1', quantity: 1, price: 1},
		{title: '商品2', quantity: 2, price: 2},
		{title: '商品3', quantity: 3, price: 3}
	];

	$scope.totalCart = function(){
		var total = 0;
		for(var i=0, len=$scope.items.length; i<len; i++){
			total = total + $scope.items[i].price * $scope.items[i].quantity;
		}

		return total;
	};
	
	$scope.subtotal  = function(){
		return $scope.totalCart() - $scope.bill.discount;
	};

	function calculateDiscount(newValue, oldValue, scope){
		$scope.bill.discount = newValue >100 ? 10 : 0;
	};

	$scope.$watch($scope.totalCart, calculateDiscount);

}
```

注意：CartController的底部，我们再totalCart()的值上面设置了一个监控，用来计算此次购物的总价。只要这个值发生变化，监控器就会调用calculateDiscount()，然后我们就可以把折扣设置为相应的值。如果总价超过100美元，我们将会把折扣设置为10美元。否则，折扣为0。

效果图如下：

![](http://7xtoaz.com2.z0.glb.clouddn.com/xigua_angular_006)

完整代码如下：

```
<html>
	<head>
		<meta charset="UTF-8">
		<title>带折扣的购物车</title>
	</head>
	<body ng-app="myApp" ng-controller="CartController">
		<h1>Your Order</h1>
		<div ng-controller="CartController">
			<div ng-repeat="item in items">
				<span>{{item.title}}</span>
				<input ng-model="item.quantity">
				<span>{{item.price | currency}}</span>
				<span>{{item.price * item.quantity | currency}}</span>
			</div>
			<div>Total:{{totalCart() | currency}}</div>
			<div>Discount:{{bill.discount | currency}}</div>
			<div>Subtotal:{{subtotal() | currency}}</div>
		</div>
		<script src="../angular.js"></script>
		<script>
			var app = angular.module('myApp', []);
			app.controller('CartController', function CartController($scope) {
				$scope.bill = {};

				$scope.items = [
					{title: '商品1', quantity: 1, price: 1},
					{title: '商品2', quantity: 2, price: 2},
					{title: '商品3', quantity: 3, price: 3}
				];

				$scope.totalCart = function(){
					var total = 0;
					for(var i=0, len=$scope.items.length; i<len; i++){
						total = total + $scope.items[i].price * $scope.items[i].quantity;
					}

					return total;
				};
				
				$scope.subtotal  = function(){
					return $scope.totalCart() - $scope.bill.discount;
				};

				function calculateDiscount(newValue, oldValue, scope){
					$scope.bill.discount = newValue > 100 ? 10 : 0;
				};

				$scope.$watch($scope.totalCart, calculateDiscount);
			});
		</script>
	</body>
</html>
```

