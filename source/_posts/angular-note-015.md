---
title: 015-watch中的性能注意事项-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-12 12:43:09
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

### watch()中的性能注意事项

前面的例子可以正确的运行，但是却存在潜在的性能问题。如果你在totalCart()中打一个调试断点，你就会发现在渲染这个页面时，该函数被调用了6次。虽然在当前这个应用中它所引起的性能问题并不明显，但是在更加复杂的应用中，运行6次就会成为一个问题。

为什么是6次呢？其中3次我们很同意跟踪到，因为在一下每种情况下它都会运行一次：

+ 模板{ {totalCart() | currency} }
+ subtotal()函数
+ $watch()函数

然后Angular把以上整个过程又重复了一遍，最终就是6次。Angular这样做的目的是，检测模型中的变更已经被完整地进行了传播，并且模型已经被设置好。Angular的做法是，把所有被监控的属性都拷贝一份，然后把他们和当前的值进行比较，看看是否发生了变化。实际上，Angular可能会把以上过程运行10次，从而确保进行了完整地传播，如果经过10次重复之后被监控的属性还在发生变化，Angular将会报错并退出。如果出现了这种情况，你可能需要解决循环依赖的问题。

虽然你现在对这个问题感到担心，但是当你读完本书之后，这可能就不是个问题了。由于Angular需要用JavaScript实现数据绑定，所以我们与TC39团队一起开发了一个叫做Object.observe()的底层本地化实现。实现了这一点之后，Angular将会在所有存在此实现的地方自动使用Object.observe()，让你的数据绑定操作就像本地化代码一样快速。

正如你将会在下一章中所看到的，Angular带有一个很好的Chrome调试插件Batarang，它会自动为你指出哪些数据绑定操作比较昂贵。

有几种方法可以解决这个问题。一种方法是监控items数组的变化，然后重新计算$scope属性中的总价、折扣和小计值。

为了实现这一点，我们需要把模板修改一下以便使用这些属性：

```
<div>Total:{{bill.total | currency}}</div>
<div>Discount:{{bill.discont | currency}}</div>
<div>Subtotal:{{bill.subtotal | currency}}</div>
```

然后，在JavaScript中，我们将会监控items数组，当数组发生任何变化时调用一个函数来计算总价，示例如下：

```
function CartController($scope){
	$scope.bill = {};

	$scope.items = [
		{title: '商品1', quantity: 1, price: 1},
		{title: '商品2', quantity: 2, price: 2},
		{title: '商品3', quantity: 3, price: 3},
	];

	var calculateTotals = function(){
		var total = 0;
		for( var i=0, len=$scope.items.length; i<len; i++){
			total = total + $scope.items[i].price * $scope.items[i].quantity;
		}

		$scope.bill.totalCart = total;
		$scope.bill.discount = total > 100 ? 10 : 0;
		$scope.bill.subtotal = total - $scope.bill.discount;
	}

	$scope.$watch('items', calculateTotals, true);
}
```

请注意，上面的代码在调用$watch函数时把items写成了一个字符串。这样做是可以的，因为$watch函数既可以接受一个函数（就像我们前面做的），也可以接受一个字符串。如果把一个字符串传递给了$watch函数，它将会在被调用的$scope作用域中当成表达式来执行。

对于你的应用来说，这种策略可能会工作的很好。但是，既然我们在监控items数组，Angular就会制作一份数组的拷贝，用来进行比较操作。对于大型的items数组来说，如果每次在Angular显示页面时只需要重新计算bill属性，那么性能就会好很多。通过创建一个带有watchFn的$watch函数我们就可以实现这一点，watchFn将会像下面这样来重新计算属性值：

```
$scope.$watch(function(){
	
	var total = 0;
	for( var i=0, len=$scope.items.length; i<len; i++){
		total = total + $scope.items[i].price * $scope.items[i].quantity;
	}
	$scope.bill.totalCart = total;
	$scope.bill.discount = total > 100 ? 10 : 0;
	$scope.bill.subtotal = total - $scope.bill.discount;
});
```

### 监控做个东西

如果你想监控多个属性或者对象，并且当其中任何一个发生变化时就去执行一个函数，应该怎么做呢？你有两种基本的选择：
+ 监控把这些属性连接起来之后的值。
+ 把他们放到一个数组或者对象中，然后给deepWatch参数传递一个true值。

在第一种情况下，如果在你的作用域中存在一个things对象，它带有两个属性a和b，当这两个属性发生变化时都需要执行callMe()函数，你可以同时监控这两个属性，示例如下：

```
$scope.$watch('things.a + things.b', callMe(...));
```

当然，a和b也可以属于不同的对象，只要需要，这个列表可以无限长。如果列表非常长，你就需要写一个函数来返回连接之后的值，而不是依赖一个表达式来完成这一逻辑。

在第二种情况下，需要监控things对象上的所有属性，你可以这样做：

```
$scope.$watch('things', callMe(...), true);
```

这里，给第三个参数传递了一个true，请求Angular遍历things的属性，然后当其中任何一个属性发生变化时就调用callMe()。这一机制在数组上的运行方式和在对象上的运行方式相同。