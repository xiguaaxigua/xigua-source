---
title: 什么时候使用指令(Directive)、控制器(Controller)或服务(Service)？
date: 2016-05-16 10:51:26
tags: [Angular, 翻译]
---
Angular学习笔记。
<!--more-->

![](http://kirkbushell.me/assets/img/logo-angularjs.png)
## 概要
本文讲述Angular中的指令、控制器、服务的应用场景。

核心：
+ 指令：涉及到DOM操作的时候使用
+ 控制器：涉及到业务逻辑的时候使用
+ 服务：需要跨域共享数据的时候使用

翻译自[原文地址](http://kirkbushell.me/when-to-use-directives-controllers-or-services-in-angular/)。由于本人水平有限，如有任何问题，可以通过[各种方式](http://xiguaaxigua.cn/about/)找到我。

## 正文
AngularJS是一个非常强大的前端MVC框架，它引入的很多陌生的概念，例如：
+ 指令(Directive)
+ 控制器(Controller)
+ 服务(Service)

让我们来逐个分析，以理清这些概念是用来做什么的。我们来从最底层开始。

### 服务（Service)
如果你已经使用AngularJS的话，你一定遇到过服务（Service）——Angular中一个很不错的定义不是吗？Service得定期传来传去，以确保你每次处理都是同一个对象，而不像工厂模式那样。通过这一点，我们可以做一些很酷的事情，例如：使用控制器和指令影响DOM的值。有一个很常见的问题是：你怎样在你的APP的代码块之间共享数据。我们来看怎么解决这个问题：

首先，我们创建一个模块（整篇文章都会用到这个模块）。

```
var module = angular.module('my.new.module', []);
```

然后，我们创建一个新服务，假设之前的模块的功能是用来管理书籍，那么，我们来创建一个书的服务，并配置一些假数据。

```
module.service( 'Book', [ '$rootScope', function( $rootScope ) {
	var service = {
		books: [
			{ title: "西游记", author: "吴承恩" },
			{ title: "编程", author: "沈从文" }
		],

		addBook: function ( book ) {
			service.books.push( book );
			$rootScope.$broadcast( 'books.update' );
		}
	}
	return service;
}]);
```

显然，这是一个非常简单的服务（但有时候这也是你所需要的），我们所要做的是管理“图书数组”，如果需要的话，也可以自定义一个方法添加书籍。这也向我们的APP中注入了一个事件，通过我们服务告诉APP所有的数据已经被更新——事实上，它们也应该被更新。现在，我们要做的是通过服务使用控制器、指令、过滤器或者其他需要的东西——它们可以访问到服务的方法和属性。下面看代码：

```
var ctrl = [ '$scope', 'Book', function( scope, Book ) {
	scope.$on( 'books.update', function( event ) {
			scope.books = Book.books;
		});

		scope.books = Book.books;
	}
];

module.controller( "books.list", ctrl );
```

显然，我们是在为我们的服务创建一个控制器。注意，我们做了什么？之前我们在Book服务创建的“图书数组”，讲道理应该叫做ctrl控制器的作用域对象，不是吗？

这玩意儿有什么用呢？我们可以偷个懒，直接在控制器中创建数组。显而易见我们是对的，这样节省了很多时间，但如果我们想要在其他地方处理这个“图书数组”呢？通过作用域管理数据是不明智的，作用域很容易被其他东西（控制器、指令、模块等）污染和破坏。通过服务去操作数据不仅更加清洁（指不会污染$scope），也更容易控制app的增长（指app变得复杂）。这种方式让你的代码分成一块一块的，易于管理。每当有一个新项目加入的时候，你需要的是添加一个服务，而不是写各种控制器、指令、过滤器。

那么，什么时候使用服务呢？答案是，<b>当我们需要跨域共享数据的时候</b>。此外，Angular的依赖注入机制让这两者变得简单和干净。

### 控制器(Controller)
我们来谈一下控制器。

如果你是一个服务器端程序员，而且没有搞过前端MVC的话，从服务器端转移到前端的会很揪心不是吗（指前端没有MVC一说）？前端的控制器虽然和服务器端有着类似的效果（处理业务逻辑），但是也不尽相同。Angular中的控制器本身不处理“请求”，但它可以处理“路由”（很多人通过创建一个控制器来管理路由）。讲真的，控制器是用来控制app界面的，每个控制器只控制界面的一小部分。(⊙﹏⊙)b...

控制器应该被纯粹的用于管理服务、管理依赖和管理其他对象，并将它们传递给视图。当你的视图中有复杂的业务逻辑时，也应该使用控制器。在之前的Book例子中，并没有需要使用控制器的地方。

如果我想要添加一本书呢？我应该在控制器中添加一个方法吗？不，这属于DOM交互/修改的部分，我们应该把它放在指令(Directive)中。

### 指令(Directive)
我已经使用Angular写了很多应用，到目前为止，app中最复杂的部分是指令。Angular中有很强大的修改DOM的工具，也就是这部分要讲的指令。我们将在上面提到的Book例子中添加一个按钮，让用户可以自己添加书籍。

一个常见的错误是添加一个DOM交互控制器。Angular定义了很多指令去操作DOM。让我们通过这种方式扩展一下上面的例子。

```
module.directive( "addBookButton", [ 'Book', function( Book ) {
  return {
    restrict: "A",
    link: function( scope, element, attrs ) {
      element.bind( "click", function() {
        Book.addBook( { title: "Star Wars", author: "George Lucas" } );
      });
    }
  }
}]);
```

很明显，我们在创建一个指令，它唯一的目的是简单的添加一本书到“图书数组”。下面我们在视图中使用它：

```
<button add-book-button>添加</button>
```

正如你看到的，我们只是简单的讲指令添加到button的属性，每当我们点击这个按钮的时候，就会添加一本书（这里使用写死的数据“天龙八部”）。简单、容易、模块化又易于管理。

来思考一个问题，我们为什么不像下面这段代码一样写一个addBook控制器呢？

```
$scope.addBook = function() {
	Book.addBook( { title: "天龙八部", author: "金庸" } );
};
```

这段代码也会达到同样的效果，不是吗？那如果在另外一个位置，也想要使用同样的功能呢？我是不是该copy一份？讲真的，绝逼不能这样。通过指令去创建的话，我们不用担心以后会不会用到，如果用到的话，在需要的地方加个add-book-button属性就可以了。当需要操作DOM的时候，通过创建指令，我们可以很容易处理逐渐复杂的应用程序。这简直太好了，我们不必纠结于怎么实现，而仅仅写个指令就ok。

Angular模块化开发的理念，能帮助我们创建出优秀的应用，它让我们在写前端代码的时候，不至于掉进太多的坑。通过框架的形式，可能是Angular最大的优势。

