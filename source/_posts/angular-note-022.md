---
title: 022-使用AngularJS进行开发-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-15 11:25:50
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要
到目前为止，我们已经对AngularJS的组成结构做了深入的研究。现在我们已经知道如何收集用户数据并传入应用中个，如何显示文本，以及如何使用校验、过滤器，甚至修改DOM的方法，从而可以实现一些非常有趣的事情。但是，我们应该如何把这些东西整合到一起呢？

在本章中，我们将会涵盖以下内容：

+ 在快速开发过程中如何对AngularJS应用进行布局
+ 启动服务器，查看AngularJS应用的实际运行情况
+ 使用Karma编写并运行单元测试和场景测试
+ 为产品级部署编译并压缩AngularJS应用
+ 简化部署流程（从创建新文件到运行应用和测试用例）
+ 把AngularJS和RequireJS（一个依赖关系管理库）集成到一起

## 正文

### 项目结构
我们推荐使用Yeoman（http://yeoman.io/）开发项目，它可以自动创建所有必须的文件以便启动你的AngularJS应用。

Yeoman是一款强大的工具，它由很多框架和客户端类库组成。它提供了一个快速开发环境，对那些启动和开发应用所需要的日常工作进行了自动化处理。在本章中，我们将会用一个小节的内容来讲述如何安装和使用Yeoman，然后，我们将会简单摸索一下Yeoman命令，这些命令可以用来手动执行那些自动化的操作。

如果你不想使用Yeoman，我们还会详细解释另外一些细节，因为Yeoman在Window系统上确实存在一些问题，并且安装这款软件也是一个不小的挑战。

对于那些不适用Yeoman的人，我们将会学习一个简单的应用结构（可以在GitHub示例库的chapter3/sample-app文件夹中找到这个例子），这个例子遵守了推荐的项目结构，与Yeoman生成的结构相同。应用中的文件夹可以分为以下几种类型。

#### JS源文件
请查看app/scripts文件夹，它就是用来存放所有JS源代码的地方。其中有一个主文件（app/scripts/app.js）将会为应用创建Angular模块和路由。

另外，还有一个独立的文件夹——app/scripts/controller——用来容纳独立的控制器，控制器的作用是提供动作并给scope对象发布数据，这些数据将会被显示在视图中。一般来说，控制器与视图是一一对应的。

在app/scripts中还可以找到指令、过滤器及服务；他们可以放在同一份文件夹中，但是如果它们很大很复杂也可以存成独立的文件。

#### HTML型的Angular模板文件
在app/views文件夹中，可以找到Yeoman所创建的每一个AngularJS模板片段。这个文件夹中的文件大部分都是app/scripts/controller文件夹的镜像。

还有一个重要的Angular模板文件，也就是主文件app/index.html，这个文件负责加载为应用创建的所有Angular源文件。

如果你创建了一份新的JS文件，请确保它添加到了index.html中，同时更新了主模块和路由（Yeoman会自动完成这件事情）。

#### JS库依赖
Yeoman提供了一个app/scripts/vendor文件夹，用来存放所有依赖的JS源码。想在你的应用中使用[Underscore](http://documentcloud.github.io/underscore/)或者SocketUO(http://socket.io/)吗？没问题——只要把依赖的文件添加到vendor文件夹（以及index.html文件夹中）就可以了。

#### 静态资源
我们最终都会创建的是一个HTML应用，所以它必定会带有一些CSS样式和图片，这些东西是应用的一部分。app/styles和app/img文件夹就是为此而设计的。只要把你需要用到的东西添加进去，然后在应用中引用它们即可（当然需要使用正确的相对路径）。

<i>Yeoman默认不会创建app/img路径</i>

#### 单元测试
测试极其重要，而且对于AngularJS来说测试很简单。测试时，test/spec文件夹中的内容将会与app/scripts文件夹一一对应。每一份源码文件都应该有一份对应的镜像描述文件，其中包含了用来对源码文件做单元测试的内容。种子将会为每一份控制器文件创建一个存根文件，并存放到test/spec/controllers目录下，文件名与控制器名称相同。这些都是Jasmine-style的描述文件，对于期望控制器所做的每一个行为，这份文件都提供了一个规范的描述。

#### 集成测试
AngularJS提供了对端到端（end-to-end）测试的支持，这项功能被构建到了库里面。所有的E2E测试都会以Jasmine描述文件的形式存放在tests/e2e文件夹中。

<i>Yeoman默认不会创建tests/folder目录。</i>

<i>E2E测试看起来与Jasmine类似，但实际上不同。E2E测试将会是异步执行的函数，它通过Angular Scenario Runner（Angular场景执行器）来执行。所以，不要指望能够做哪些在普通的Jasmine测试中做的事情（例如在遍历过程中使用console.log打印当前的值）。</i>

#### 配置文件
一共需要两份配置文件，第一份是karma.conf.js，这份文件是由Yeoman自动生成的，用来运行单元测试；第二份是karma.e2e.conf.js，Yeoman不会自动生成这份文件，这些文件是用来运行场景测试的。在本章结尾“与RequireJS集成”一节中还会有一份配置文件。那份文件将会描述使用Karma运行单元测试时所依赖的内容及文件。默认情况下，Karma服务器将会运行在9876端口。