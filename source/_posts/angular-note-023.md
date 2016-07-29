---
title: 023-一款菜谱应用剖析-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-15 21:12:30
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要
GutHub是一款简单的菜谱管理应用，我们设计它的目的有两个，一是用来存储美味的菜谱，二十来展示AngularJS应用的方方面面。这款应用的特性如下：
+ 两列布局
+ 在左侧有一个导航条
+ 允许你创建新菜谱
+ 允许你浏览现有的菜谱列表

主视图位于右侧，主视图将会根据具体的URL刷新，可以显示菜谱列表、菜谱项目详情，以及用来添加或编辑菜谱的菜单。下图是这款应用的截图：

## 正文
在深入分析这款应用之前，我们先来用一两段的篇幅讨论一下应用中的这三种东西是如何互相协作的，应该用什么样的视角来看待它们。

### 模型、控制器和模板之间的关系
<b>模型就是真理。</b>

你的整个应用都是由模型驱动的——视图中所展示的内容是模型，被存储起来的内容是模型，几乎所有一些都是模型！所以请花一点时间来思考一下模型，思考一下对象中的属性应该是怎样的内容、应该如何从服务端获取模型，以及如何保存它。通过数据绑定技术，视图会根据数据模型自动刷新，所以模型总是应用中的焦点。

<b>控制器将会负责业务逻辑</b>：应该如何获取模型、可以在模型上执行何种操作、视图需要模型上的何种信息，以及应该如何转换模型以获取想要的信息。表单校验任务、对服务器的调用、使用正确的数据启动视图，以及与此相关的几乎所有事情都是控制器的职责。

最后，模板代表模型的展现形式，以及用户应该如何与应用进行交互。模板主要用来做一下事情：
+ 展示模型
+ 定义用户于应用之间的交互方式（点击、输入项等）
+ 给应用提供样式，并且判断何时以及怎样显示一些元素（显示还是隐藏，鼠标滑过时的效果，等等）
+ 过滤并格式化数据（既有输入又有输出）

注意，Angular中的模板不是MVC设计模式中的必备组件。视图是模板编译并执行之后的版本，视图是模板和数据模型融合之后产生的东西。

模板中不应该包含任何形式的业务逻辑或者行为，只有控制器才可以具备这些特性。保持模板简单可以把关注点进行适当的分离，并且只使用单元测试就可以让测试工作中覆盖大部分代码。模板必须进行场景测试。

但是你可能会问，DOM操作应该放在那里呢？DOM操作并不会真正发生在控制器或者模板中。它由AngularJS指令负责（有时候也可以通过服务进行操作，DOM操作放在服务中可以避免重复的代码），在GunHub例子中也会包含这样的例子。

### 模型
对于当前这款应用，我们将会让模型保持超级简单。这里的模型就是一些菜谱，他们将会是整个应用中唯一的模型对象，其他所有东西都构建在这个模型之上。

每一条菜谱都具有一下属性：
+ 一个ID，这个ID将会被持久化到服务器端
+ 一个名称
+ 一个简短的描述
+ 烹饪指南
+ 是否是特色菜谱
+ 配料数组，每一项都包含重量、单位及名称

就这么多，超级简单，应用中的所有东西都围绕着这个简单的模型而构建。下面是一条简单的菜谱：

```
{
	"id": "1",
	"title": "西红柿炒鸡蛋",
	"description": "很好吃哦，健康又美味",
	"ingredients": [							//成分
		"amount": "1",
		"amountUnits": "份",					//计量单位
		"ingredientName": "西红柿、鸡蛋"		//成分名称
	],
	"instruction": "1.买一个西红柿\n" + "2.买一个鸡蛋\n" + "3.炒\n"	
}
```

我们继续来看，围绕这个简单的模型如何构建更加复杂的UI功能。

### 控制器、指令及服务
首先，我们来看一看指令和服务代码，然后再来看看这款应用所需要的几个控制器。

<b>服务</b>

以下内容位于app/scripts/services/services.js中

```
'use strict';

var services = angular.module('guthub.services', ['ngResource']);

services.factory('Recipe', ['$resource',
    function($resource) {
        return $resource('/recipes/:id', { id: '@id' });
    }
]);

services.factory('MultiRecipeLoader', ['Recipe', '$q',
    function(Recipe, $q) {
        return function() {
            var delay = $q.defer();
            Recipe.query(function(recipes) {
                delay.resolve(recipes);
            }, function() {
                delay.reject('Unable to fetch recipes');
            });
            return delay.promise;
        };
    }
]);

services.factory('RecipeLoader', ['Recipe', '$route', '$q',
    function(Recipe, $route, $q) {
        return function() {
            var delay = $q.defer();
            Recipe.get({ id: $route.current.params.recipeId }, function(recipe) {
                delay.resolve(recipe);
            }, function() {
                delay.reject('Unable to fetch recipe ' + $route.current.params.recipeId);
            });
            return delay.promise;
        };
    }
]);
```

我们首先来看服务。

在这份文件中，我们实例化了3个Angular服务。

其中有一个菜单服务，它所返回的东西叫做Angular Resource。这些都是RESTful型（表征状态转移）的资源，它们会指向一个RESTful服务器。Angular Resource将会封装底层的$http服务，所以你的代码只要负责处理对象就可以了。

只需要一行简单的代码——return $resource（当然，还需要依赖guthub.services模块），我们就可以把菜谱作为参数传递给任何控制器了，然后菜谱对象就会被注入到控制器中。这样一来，每一个菜谱对象就都具备了下面这些内置的方法：

+ Recipe.get()
+ Recipe.save()
+ Recipe.query()
+ Recipe.remove()
+ Recipe.delete()

<i>如果你打算使用Recipe.delete，并且希望应用能在IE中使用，你必须这样调用它：Recipe[delete]()，这是因为delete是IE中的关键字。</i>

对于单个菜谱来说，除了查询工作之外，其余所有工作都可以通过以上方法进行；默认情况下，query()将会返回一个菜谱数组。

声明资源的那一行代码——return $resource——还会替我们做如下一些事情：

1.请注意URL中的:id，此属性被设置给了RESTful资源。这样做的基本含义是，当进行查询操作时（如Recipe.get()），如果传递进来的对象带有一个id字段，那么这个字段的值将会被自动添加到URL后面。

也就是说，调用Recipe.get({id: 5})将会访问/recipe/5路径。

2.那么第二个对象{id: @id}呢？正如人们所说的，一行代码需要数千行注释，所以我们还是来看一个例子吧。

假设我们有一个菜谱对象，必要的信息都已经放在这个对象里面了，包括id。

然后，通过下面这些代码我们就可以把它曹村器来：

```
//假设existingRecipeObj中包含了所有必需的字段
//包括id字段(假如id为14)

var recipe = new Recipe(existingObj);
recipe.$save();
```

以上代码将会向/recipe/14路径发起了一次POST请求。

@id会告诉请求应该从对象中获取id字段，然后把它用作id参数。这是一种额外的快捷方式，可以省略很多行代码。

在apps/scripts/services/services.js中还有另外两个服务，这两个服务都是加载器（Loader）：一个用来加载单个菜谱（RecipeLoader），另一个用来加载所有菜谱（MultiRecipeLoader）。当我们连接到路由上时，就会用到这两个服务。它们的核心工作原理非常类似。这两个服务的工作流程如下。
+ 1.创建一个延迟对象$q（这些都是AngularJS中的promise，用来对异步函数进行链式调用）。
+ 2.向服务端发起一次调用
+ 3.在服务端返回数据后解析延迟对象
+ 4.返回promise，AngularJS中的路由机制将会使用这个对象