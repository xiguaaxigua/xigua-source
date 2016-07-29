---
title: 006-Angular中的文本和表单-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-09 10:01:42
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要
+ 1.显示文本的两种方法
 + <b>双花括号插值语法</b>和<b>ng-bind</b>
 + 相同点：作用相同
 + 不同点：在数据加载完成前，ng-bind方法不会在UI中显示
+ 2.Angular里的表单
 + 新的指令：ng-submit（自定义submit，并阻止默认的POST方法）
 + 新的属性：ng-change（指定一个控制器方法，数据改变时调用）
 + 监视器：$watch()函数

## 正文

### 显示文本
使用ng-bind指令，你可以在UI中的任何地方显示并刷新文本。它还有两种等价形式。第一种是使用花括号的形式，我们已经见过了：

``` html
<p>{{greeting.text}}</p>
```

还有一种就是使用基于属性的指令，叫做ng-bind:

``` html
<p ng-bind="greeting"></p>
```

对于输出的内容来说两种形式是等价的。如果模型变量greeting被设置为“Hi there”，Angular将会生成以下HTML：

``` html
<p>Hi there</p>
```

然后浏览器就会显示“Hi there”。

那么，你为什么要使用一种方式代替另一种呢？我们创建双花括号插值语法的目的是为了读起来更加自然，并且需要输入的内容更少。虽然两种形式的输出内容相同，但是使用双花括号语法的时候，在Angular使用数据替换这些花括号之前，第一个加载的页面，也就是应用中的index.html，其未被渲染好的模板可能会被用户看到。而使用第二种方法的视图不会遇到这个问题。

造成这种现象的原因是，浏览器需要首先加载HTML页面，渲染它，然后Angular才有机会把它解释成你期望看到的内容。

好消息是，在大多数模板中你依然可以使用双花括号插值语法。但是，对于index.html页面中的数据绑定操作，建议使用ng-bind。这样一来，在数据加载完成之前你的用户就不会看到任何内容。

### 表单输入

在Angular中使用表单元素非常方便。正如我们在前面几个例子中看到的，你可以使用ng-model属性把元素绑定到你的模型属性上。这一机制对于所有标准的表单元素都可以起作用，例如文本框、单选按钮、复选框，等等。我们可以像下面这样把一个复选框绑定到一个属性上：

``` html
<form ng-controller="SomeController">
	<input type="check" ng-model="youCheckedIt">
</form>
```

这样做的含义是：
+ 1.当用户选中了复选框之后，SomeController中的$scope的youCheckedIt的属性就会变成true。而反选复选框会让youCheckedIt变为false。
+ 2.如果你在SomeController中把$scope.youCheckedIt设置为true，那么UI中的复选框将会变成选中状态。设置为false将会反选复选框。

举例来说，当用户做了某件事情的时候，我们希望程序能真正的做出某种动作。对于输入元素来说，你可以使用ng-change属性来指定一个控制器方法，一旦用户修改了输入值，这个方法就会被调用。我们来做一个简单的计算，帮助消费者计算一下需要付多少钱：

``` html
<form action="" ng-controller="StartUpController">
	Starting: <input type="text" ng-change="computeNeeded()" ng-model="funding.startingEstimate">
	Recommendation:{{funding.needed}}
</form>
``` 

对于这个非常简单的例子来说，我们只要把输入文本框的值设置为用户估价的10倍即可。同时，在一开始的时候我们会把文本框的默认值设置为0：

``` javascript
function StartUpController($scope){
	$scope.funding = {startingEstimate : 0};

	$scope.computeNeeded = function(){
		$scope.funding.needed = $scope.funding.startingEstimate * 10;
	}
}
```

但是以上代码所采取的策略有一个潜在的问题，即，只有当用户在文本框输入值的时候我们才会去计算所需的金额。当用户在这个特定的输入框中输入时，输入框就会正确的刷新。但是，如果还有其他输入框也绑定到模型中的这个属性上，会怎么样呢？如果接受到服务端的数据，导致数据模型进行刷新，又会怎么样呢？

为了能够正确地刷新输入框，而不管它是通过何种途径进行刷新的，我们需要使用$scope中的$watch()的函数。在本章后续的内容里将会详细讨论这个watch函数。最基本的一点就是，<b>你可以调用$watch()函数来监视一个表达式，当这个表达式发生变化时就会调用一个回调函数</b>。

在当前这个例子中，我们需要监视funding.startingEstimate，当它的值发生变化时就调用computeNeeded()。下面运用这一技术重写了StartUpController：

``` javascript
function StartUpController($scope){
	$scope.funding = {startingEstimate : 0};

	$scope.computeNeeded = function(){
		$scope.funding.needed = $scope.funding.startingEstimate * 10;
	}

	$scope.$watch('funding.startingEstimate', computeNeeded);
}
```

请注意，<b>需要监视的表达式位于引号内</b>。是的，它是一个字符串。这个字符串会被当做Angular表达式来执行。表达式可以执行一些简单的操作，并且可以访问$scope对象中的属性。在本章后续的内容中我们将会详细解释表达式相关的内容。

我们还可以监视一个函数的返回值，但是监视funding.startingEstimate属性是没有用的，因为这个值算出来是0，也就是它的初始值，而且这个值永远不会发生变化。

然后，无论何时只要funding.startingEstimate发生变化，我们的funding.needed就会自动刷新，我们可以编写一个更加简单的模板，示例如下：

``` html
<form action="" ng-controller="StartUpController">
	Starting: <input type="text" ng-model="funding.startingEstimate">
	Recommendation: {{funding.needed}}
</form>
```

在某些情况下，你不想一有变化就立刻做出动作；而是要进行等待，直到用户告诉你他已经准备好了，例如完成订购或者发出一条确认信息之后。

如果你正在使用表单把输入项组织起来，你可以在form自身上使用ng-submit指令来指定一个函数，当表单提交的时候可以执行这个函数。我们来扩展前面的例子，实现这样一个功能，当用户点击按钮的时候，就计算出他所选商品的总金额：

``` html
<form action="" ng-submit="requestFunding()" ng-controler="StartUpController">
	Starting: <input type="text" ng-change="computeNeeded()" ng-model="startingEstimate">
	Recommendation:{{needed}}
	<button>Fund my StartUp!</button>
</form>
<script>
	function StartUpController($scope){
		$scope.computeNeeded = function(){
			$scope.needed = $scope.startingEstimate * 10;
		};

		$scope.requestFunding = function(){
			window.alert('Sorry, please get more customers first.')
		};
	}
</script>
```

同时，在提交表单的时候，ng-submit还会自动阻止浏览器执行默认的POST操作。

对于处理其他事件的情况，例如提供非表单提交型的交互，Angular提供了一些事件处理指令，它们类似于浏览器原生的事件属性。对于click，可以使用ng-click；对于ondbclick，可以使用ng-dbclick；等等。

我们可以试着实现一个这样的功能：再次扩展开始的那个计算器，为它添加一个复选框按钮，这个按钮将会把输入值重置为0：

``` html
<form action="" ng-submit="requestFunding()" ng-controler="StartUpController">
	Starting: <input type="text" ng-change="computeNeeded()" ng-model="startingEstimate">
	Recommendation:{{needed}}
	<button>Fund my StartUp!</button>
	<button ng-click="reset()">Reset</button>
</form>
<script>
	function StartUpController($scope){
		$scope.computeNeeded = function(){
			$scope.needed = $scope.startingEstimate * 10;
		};

		$scope.requestFunding = function(){
			window.alert('Sorry, please get more customers first.')
		};

		$scope.reset = function(){
			$scope.startingEstimate = 0;
		};
	}
</script>
```
