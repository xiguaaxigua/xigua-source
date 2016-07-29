---
title: 024-菜谱应用中的指令-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-18 11:13:02
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要
本文讲述菜谱中用到的指令。

## 正文

### 指令
现在，我们可以来看看应用中的那些指令了。在目前这款应用中将会用到如下两条指令。

<b>butterbar</b>

当路由发生变化同时页面还在加载信息时，这一指令将会进行显示和隐藏信息的操作。指令会被嵌入到路由变化机制中，然后根据页面的状态自动隐藏或者显示其标签中的内容。

<b>focus</b>

focus指令用来确保特定的输入项（或者元素）能够获得焦点。

我们来看代码：

```
//以下代码位于app/scripts/directives.js文件中
'use strict';

var directives = angular.module('guthub.directives', []);

directives.directive('butterbar', ['$rootScope',
    function($rootScope) {
  return {
    link: function(scope, element, attrs) {
      element.addClass('hide');

      $rootScope.$on('$routeChangeStart', function() {
        element.removeClass('hide');
      });

      $rootScope.$on('$routeChangeSuccess', function() {
        element.addClass('hide');
      });
    }
  };
}]);

directives.directive('focus',
    function() {
  return {
    link: function(scope, element, attrs) {
      element[0].focus();
    }
  };
});
```

以上指令将会返回一个对象，这个对象只有一个link属性。在以后我们将会深入学习如何创建你自己的指令，目前来说，你需要知道下面这些内容就可以了：
+ 1.指令的处理过程分为两个步骤。在第一步（编译阶段）中，找到绑定在DOM元素上的所有指令，然后进行处理。所有对DOM的操作都发生在编译阶段，在这一阶段结束之后，会产生一个内联的函数。
+ 2.在第二步（链接阶段，也就是上面代码所使用的阶段）中，第一步中所产生的DOM模板会被链接到作用域上。同时，会根据需要添加监控器（watches）或者监听器（listeners），从而作用域（scope）和元素之间进行动态绑定。这样一来，与作用域相关的左右内容都是在链接阶段进行的。

那么指令内部会发生什么呢？我们来看一看：

```
<div butterbar>My Loading Text...</div>
```

在一开始的时候，他只是简单的把元素隐藏起来，然后在跟作用域是添加两个监控器。每当路由发生变化时，它就会显示内部的元素（通过修改元素的样式类）；每当路由成功完成变化之后，它又会把butterbar隐藏起来。

另外还有一个有趣的东西值得注意，那就是如何把$rootScope注入到指令中。所有指令都会被直接连接到AngularJS的依赖注入系统中，所以你可以把服务或者任何你需要的东西注入进去。

最后一件值得注意的事情就是用来操作元素的API。AngularJS使用类似jQuery的语法（addClass、removeClass），这令jQuery老手很高兴。AngularJS实现了一个jQuery调用的自己，所以对于AngularJS项目来说，jQuery是一个可选的依赖库。如果你最终会在项目中使用完整的jQuery库，那么你需要知道，AngularJS将会用完成的jQuery库来代替内置的加jQlite实现。

第二个指令focus更加简单。它只是调用一下当前元素上的focus()方法而已。你可以在任何输入元素上添加focus属性来调用它，示例如下：

```
<input type="text" focus>
```

在页面加载完成之后，这个元素就会立即获得焦点。