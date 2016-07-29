---
title: 003-AngularJS简介-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-04 09:41:57
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要
+ Angular简介
+ Angular涉及到的常用概念
	+ 客户端模板
	+ MVC
	+ 数据绑定
	+ 依赖注入
	+ 指令

## 正文
### 本节目录
-- [客户端模板](#Client-template)
-- [Model View Controller（MVC）](#MVC)
-- [数据绑定](#DataBind)
-- [依赖注入](#DI)
-- [指令](#Directive)
 
### AngularJS简介
目前，在创建优质的Web应用方面，我们已经具备了令人难以置信的能力。但是，在创建这些应用的过程中所引入的复杂性同样令人难以置信。我们Angular团队致力于减轻开发人员在开发AJAX应用过程中的痛苦。在Google，我们已经通过艰苦的努力构建了很多大型的web应用，例如Gmail、Maps、Calender等。我们认为，这些经验可以让每个人获益。

在编写web应用的过程中，我们都希望一边编写代码，一边就能够惊奇的看到代码所产生的效果。我们希望编码的过程就像是创作，而不是为了满足web浏览器内部的一些奇怪的运行机制。

我们还想要一个开发环境，它可以帮助我们在设计方面做出抉择，让应用从一开始就易于创建和理解；同时，当应用变得很大的时候，还能够持续做出正确的抉择，让我们的应用易于测试、扩展和维护。

在AngularJS框架中，我们已经成功实现了以上目标。我们对所取得的成就感到十分兴奋。这里面很大一部分功劳要归于Angular周边的开源社区，它们互相支持，做的非常出色，同时也教会我们很多东西。希望你也能加入我们的社区，把Angular发展的更好。

在Github库中有一些大型的、复杂的例子，以及一些代码片段，你可以查看这些例子、创建分支。或者在我们的Github页面[http://github.com/shyamseshadri/angularjs-book](http://github.com/shyamseshadri/angularjs-book)上尝试运行它们。

### 一些概念
你将会用到一些核心的概念，这些概念将会贯穿整个应用。并非我们发明了这些概念，而只是从其他开发环境中大量地借用了这些比较成功的习惯用语，然后以适合HTML、浏览器及其他常见的web标准的方式来实现了这些概念。

#### <a id="Client-template"></a>客户端模板
多页面web应用会在服务器端创建HTML，把HTML和数据装配并混合起来，然后再把生成的页面发送到浏览器中。在某种程度上，大部分单页面应用——也叫做AJAX应用——也会做同样的事情。在这一方面，在Angular中，模板和数据都会发送到浏览器中，然后在客户端进行装配。这样一来，服务器的角色就变成了仅仅为这些模板提供一些静态的资源，然后为这些模板提供所需要的正确数据。

我们来看一个例子，看看在Angular中浏览器是如何把数据和模板装配起来的。我们会例行公事地写出一个“Hello World”示例，但是不会把“Hello World”作为一行完整的字符串打印出来，而会把打招呼用的单词“Hello”构成一个数据，然后再在后面修改这个单词。

为了实现这个功能，我们把模板创建在hello.html中：
``` html
<html ng-app>
<head>
	<script src="angular.js"></script>
	<script src="controller.js"></script>
</head>
<body>
	<div ng-controller='HelloController'>
		<p>{{greeting.text}}, World</p>
	</div>
</body>
</html>
```
同时我们的应用逻辑写在controller.js中：
``` javascript
function HelloController($scope){
	$scope.greeting = {text: 'hello'};
}
```
使用任意浏览器打开hello.html,然后我们就可以看到如下图所示的效果：
![](http://7xtoaz.com1.z0.glb.clouddn.com/xigua_angualr_002)
与目前广泛使用的一些方法相比，这里有一些非常有趣的东西需要注意：
+ HTML里面没有class或者ID来标记在哪里添加事件监听器。
+ 当HelloController把greeting.text设置成单词Hello的时候，我们没有必要注册任何事件监听器或者编写任何回调函数。
+ HelloController只是一个普通的javascript类，并且不需要继承Angular所提供的任何东西。
+ HelloController可以获取到它所需要的$scope对象，而且没有必要去创建它。
+ 没有必要自己调用HelloController的构造方法，或者指定何时去调用它。

后面我们还会看到更过不同之处，但是这里已经能够清晰地显示出Angular应用与以前类似的应用在结构设计上存在巨大的差异。

为什么我们会在设计上做出这种选择，Angular的工作原理又是什么呢？我们来看看Angular从其他地方“偷来”的一些很好的理念。

### <a id="MVC"></a>Model View Controller（MVC）
MVC程序结构是在20世纪70年代作为Smalltalk语言的一部分被引入的。在Smalltalk的发展初期，MVC在每一种桌面开发环境中都变得非常流行，这些环境中的用户界面都相当复杂。无论是C++、Java还是Objective-C，它们都带有一些MVC的意味。但是，直到现在，MVC还依然与web开发毫不相干。

MVC背后的核心理念是：<b>你应该把管理数据的代码（model）、应用逻辑代码（controller），以及向用户展示数据的代码（view）清晰地分离开</b>。

视图会从模型中获取数据，然后展示给用户。当用户通过鼠标点击或者键盘属于与应用进行交互的时候，控制器将会作出响应并修改模型中的数据。最后，模型会通视图数据已经发生了变更，这样视图就可以刷新其中显示的内容。

在Angular应用中，视图就是Document Object Model（DOM，文档对象模型），控制器就是JavaScript类，而模型中的数据则被存储在对象的属性中。

MVC之所以非常灵活，原因有很多。首先，对于什么东西应该放在哪里，MVC给了你一个思想上的模型。所以你不需要每次都来构建这个模型。如果和你一起做项目的人知道你正在使用MVC来组织代码，那么他们立刻就能明白你写的是东西是什么意思。最重要的一点是，使用mVC模型会让你的应用更加易于扩展、维护和测试。

### <a id="DataBind"></a>数据绑定
在AJAX型的单页应用普及之前，类似于Rails、PHP和JSP之类的平台都可以帮助我们创建用户界面（UI），他们会把HTML字符串和数据混合起来，然后再发送给用户并显示。

而jQuery之类的库则在客户端继承了这一模型，让我们遵守类似的风格，但是使用jQuery可以单独刷新DOM中的局部内容，而不是刷新整个页面。在jQuery中，我们会把HTML模板字符串和数据混合起来，然后把获得的结果插入DOM中我们所期望的位置，插入的方式是把结果设置给一个占位符元素的innerHTML属性。

以上机制都工作的相当不错，但是当你想要把最新的数据插入到UI中，或者根据用户输入来修改数据的时候，你就需要做很多极其繁琐的工作来保证数据的状态是正确的，并且UI和JavaScript属性要同时正确。

但是，如果我们不需要编写代码就能做到以上所有事情会怎么样？如果我们可以仅仅声明UI中的某个部分需要映射到某个JavaScript属性，然后让他们自己去同步会怎么样？<b>这种编程风格叫做数据绑定</b>。因为他可以和MVC很好地结合起来，所以我们把它引入到了Angular中。这样一来，当你编写视图和模型的时候，可以节省代码量。在UI中，把数据从一个值修改成另一个值的大部分工作会自动进行。

为了在实战中看到这一点，我们来修改第一个例子，让它变成动态的。目前的情况是，HelloController会给模型greet.text赋一次值，之后再也不会去修改它。为了让它变成动态的，我们来修改这个例子，增加一个文本输入框，他会把greeting.text的值修改成用户所输入的内容。

下面是新的模板：

``` html
<html ng-app>
	<head>
		<script src="angular.js"></script>
		<script src="controller.js"></script>
	</head>
	<body>
		<div ng-controller="HelloController">
			<input ng-model="greeting.text">
			<p>{{greeting.text}}, World</p>
		</div>
	</body>
</html>
```
控制器HelloController保持原样不变。
``` javascript
function HelloController($scope){
	$scope.greeting = {text: 'Hello'};
}
```
把这个例子加载的浏览器中，效果如下图所示：
![](http://7xtoaz.com2.z0.glb.clouddn.com/xigua_angualr_003)
如果我们把输入框中的Hello换成Hi，可以看到效果如下图所示：
![](http://7xtoaz.com2.z0.glb.clouddn.com/xigua_angular_004)

我们没有在输入框上注册任何change事件监听器就可以让UI自动刷新了。这一机制对于来自服务器的更新同样有效，在我们的控制器中，可以向服务器发起一次请求，获取响应，然后把$scope.greeting.text设置为服务端所返回的内容。Angular会自动把输入框和花括号中的文本更新为所获得的新值。

### <a id="DI"></a>依赖注入
前面曾经提到过这些内容，但是请允许我再重复一下，对于HelloController还有很多东西不需要我们去编写。例如，进行数据绑定的$scope对象会被自动传递给我们；我们并不需要调用任何函数去创建这个对象。只要把$scope对象放在HelloController的构造函数里面，然后就可以获取它了。

正如后续章节中会看到，$scope对象并不是我们唯一可以获取到的东西。如果我们想把数据绑定到用户浏览器中的URL地址上，可以使用$location对象，只要把$location对象放到我们的构造函数中即可，示例如下：
``` javascript
function HelloController($scope, $location){
	$scope.greeting = {text: 'Hello'};
	//这里可以使用$location对象来做一些很酷的事情...
}
```

这种神奇效果是通过Angular的依赖注入机制实现的。依赖注入让我们遵守这样一种开发风格：<b>我们的类只是简单的获取他们所需要的东西，而不需要创建那些它们所需要的东西</b>。

这种风格遵守了一种叫多迪米特法则（Law of Demeter, [http://en.wikipedia.org/wiki/Law_of_Demeter](http://en.wikipedia.org/wiki/Law_of_Demeter)）的设计模式，也叫做最少知识原则。既然HelloController的职责是设置greeting模型的内部状态，那么用这一法则就意味着，他不应该去操心任何其他的东西，例如$scope是如何创建的，以及到哪里去找到这个对象。

这一特性并非仅仅对Angular框架所创建的对象有效，你可以以同样的方式去编写所有其他代码。

### <a id="Directive"></a>指令
Angular最强大的功能之一就是，你可以把模板编写成HTML的形式。之所以可以做到这一点，是一位我们引入了一款强大的DOM转换引擎，可以用它来扩展HTML的语法。

我们已经在模板里面看到了一些新的属性，这些属性不属于HTML规范。例如，
+ 我们引入了双花括号语法来实现数据绑定；
+ 引入了ng-controller用来指定每个控制器负责监视视图的哪一部分；
+ 引入了ng-model，用来把数据绑定到模型中的一部分属性上。

我们把这些叫做HTML扩展指令。

Angular内置了很多指令，可以帮助你为你的应用定义视图，我们很快会看到更多相关内容。这些指令可以把创建的视图定义成模板，他们可以设置应用的运行方式，或者用来创建可复用的组件。

同时，你并不会受限于Angular内置的指令。你可以编写自己的指令用来扩展HTML模板的功能，从而实现你梦想中 的任何东西。

