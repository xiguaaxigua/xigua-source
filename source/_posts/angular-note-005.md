---
title: 005-初设Angular-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-08 12:45:32
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要

+ Angular的调用
	+ 使用CDN
	+ 本机加载
+ ng-app的使用（标识Angular管理的DOM区域）
+ 浅谈模块机制（避免命名冲突）
+ 数据绑定机制

## 正文
使用典型的类库时，你可以选择并使用你所喜欢的功能：而对于Angular框架来说，你必须把它作为一个完整的套件来使用，框架中所有东西都包含在里面。在本章中，我们将会覆盖Angular的所有基础模块，这样你就可以理解他们是如何被装配到一起的。在后续的章节中，将会对这里涉及的很多模块做出更加详细的解析。

### 调用Angular

为了使用Angular，所有应用首先做两件事情：
+ 加载angular.js库。
+ 使用ng-app指令告诉Angular应该管理DOM中的哪一部分。

### 加载脚本

加载类库的操作非常简单，与其他JavaScript类库遵循同样的规则。你可以从Google的CDN（内容分发网络）中加载类库，示例如下：
``` javascript
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.0.4/angular.min.js"></script>
```
我们推荐你使用Google的CDN，Google的服务器非常快，并且可以再多个应用之间缓存脚本库。也就是说，如果你的用户有多款应用使用了Angular，类库只要下载一次就可以了。同时，如果用户访问了其他站点，而该站点使用了Google的CDN链接来获取Angular库，那么用户在访问你的站点时，就不需要再次下载Angular库了。

如果你更喜欢使用本地的主机，当然也可以，只需要在src属性中指定正确的地址即可。

<i>鉴于国内的特殊情况，不推荐使用Google的CDN。</i>

### 使用ng-app声明Angular的边界

你可以使用ng-app指令告诉Angular应该管理页面中的哪一块。如果你正在构建一款纯Angular应用，那么你应该把ng-app指令写在html标签中，示例如下：

``` html
<html ng-app>
	...
</html>
```

这样就会告诉Angular去管理页面上的所有DOM元素。

如果你有一款现有的应用，该应用正在用其他的一些技术来管理DOM，那么你可以让Angular只管理页面中的一部分，只要在页面中放入一些div之类的元素即可。

```html
<html>
	<div ng-app>
		...
	</div>
</html>
```

### Model View Controller

在第一章中，我们曾经提到过Angular支持Model View Controller风格的应用设计。尽管设计Angular应用具有很大的灵活性，但是一下这些风格你将会经常涉及：
+ 用来容纳数据的模型，模型代表应用当前的状态。
+ 用来展示数据的一些视图。
+ 用来管理模型和视图之间关系的一些控制器。

你将会使用对象的属性来创建数据模型，甚至只用基本数据类型来存储数据。模型属性并没有什么特殊的东西。如果你想要给用户展示一些文本，你可以使用字符串，示例如下：

``` javascript
var someText = 'You have started your journey.';
```

你可编写一个HTML页面来创建视图，然后把它和你模型中的数据融合起来。正如我们前面所看到的，你可以在DOM中插入占位符，然后像下面这样设置其中的文本：

``` html
<p>{{someText}}</p>
```

我们把这叫做双花括号插值语法，因为它可以把新的内容插入到现有的模板中。

控制器就是你所编写的类或者类型，它的作用是告诉Angular该模型是由哪些对象或者基本数据构成的，只要把这些对象或者基本数据设置到$scope对象上即可，$scope对象会被传递给控制器：

``` javascript
function TextController($scope){
	$scope.someText = someText;
}
```

把以上所有东西整合起来，我们就获得了如下内容：

``` html
<html ng-app>
	<body ng-controller="TextController">
		<p>{{someText}}</p>
		<script src="angular.js"></script>
		<script>
			function TextController($scope){
				$scope.someText = 'You have started your journey.';
			}
		</script>
	</body>
</html>
```

把以上内容加载到浏览器中，你将会看到：
You have started your journey.
在很简单的情况下，以上使用个基本数据类型的模型工作得很好，但是对于大多数应用来说，你都需要创建模型对象类来容纳你的模型。我们来创建一个messages模型对象，然后用它来存储someText属性。所以模型不再是一下内容：

``` html
$scope.someText = 'You have started your journey.';
```

而会这样编写：

``` javascript
var messages={};
messages.someText = 'You have started your journey.';
function TextController($scope){
	$scope.messages = messages;
}
```

然后在你的模板中这样使用：

``` html
<p>{{messages.someText}}</p>
```

在稍后讨论$scope对象的时候我们将会看到，像这样创建模型对象的方法可以避免一些非预期的行为，这些行为可能是由$scope对象中的原型继承机制所导致的。

在上一个例子中，我们在全局作用域中创建了一个TextController。虽然对于示例代码来说，这没有什么问题，但是其实定义控制器的正确方式是，把它定义成模块的  一部分，控制器可以为应用里面相关的部分提供命名空间机制。修改之后的代码看起来就像下面这样：

``` html
<html ng-app>
	<body ng-controller="TextController">
		<p>{{someText.message}}</p>
		<script src="angular.js"></script>
		<script>
			var myAppModule = angular.module('myApp', []);
			myAppModule.controller('TextController', 
				function TextController($scope){
					var someText = {};
					someText.message = 'You have started your journey.';
					$scope.someText = someText;
				}
			);
		</script>
	</body>
</html>
```
在这个版本中，我们把ng-app属性设置成了模块的名称myApp。然后我们再调用angular对象创建了一个名为myApp的模块，并且把控制器函数传递给了myApp模块的controller函数。

稍后我们就会来解释什么是模块以及为什么要使用模块之类的问题。现在你只要记住，把东西从全局命名空间中隔离开是一件非常好的事情，而模块机制可以帮助我们实现这一点。

### 模板和数据绑定
Angular应用中的模板只是一些HTML片段而已，我们可以从服务器商家在，或者在script标签中定义，处理方式与所有其他静态资源相同。如果你需要UI组件，你可以在模板中进行定义，使用标准的HTML加上Angular指令即可。

模板一旦加载到浏览器之后，Angular将会把它和数据整合起来，然后再把这些模板展开到整个应用中。在第一章中，当我们显示购物车中的物品时，我们已经看到过这种例子：

``` html
<div ng-repeat="item in items">
	<span>{{item.title}}</span>
	...
</div>
```

这里，对于items数组中的每一个元素，Angular将会给外出div生成一份拷贝，包括其中的所有内容。

那么，这里的数据是从哪儿来的呢？在购物车实例中，我们只是在代码中的一个数组里定义了它。在你刚开始构建UI，并且只是想测试一下它的运行效果的时候，这样能工作得很好。但是，大多数应用都会用到服务端的一些持久化的数据。浏览器中的应用将会连接到服务端，请求用户当前加载页面所需要的数据，然后Angular再把这些数据和模板融合起来。

基本的运作流程如下：
+ 1.用户请求应用起始页。
+ 2.用户的浏览器向服务器发起一次HTTP连接，然后加载index.html页面，这个页面里面包含了模板。
+ 3.Angular被加载到页面中，等待页面加载完成，然后查找ng-app指令，用来定义模板边界。
+ 4.Angular遍历模板，查找指令和绑定关系，这将触发一系列动作：注册监听器、执行一些DOM操作、从服务器获取初始化数据。这项工作的最后结果是，应用将会启动起来，并且模板被转换成了DOM视图。
+ 5.连接到服务器去加载需要展示给用户的其他数据。

对于每一个Angular应用来说，步骤1到步骤3都是标准化的，步骤4和步骤5是可选的。这些步骤可以同步进行也可以异步进行。为了提升性能，对于应用中的第一个视图，你可以把视图和HTML模板一起加载进来，从而避免发起多次请求。

使用Angular构建应用的时候，可以将应用中的模板和数据分离开来，这样就可以把这些模板缓存起来。在第一次请求之后，只需要把新的数据下载到浏览器中即可。与JavaScript、图片、CSS以及其他资源一样，把这些模板缓存起来可以提升应用的性能。

