---
title: 018-使用路由和$location切换视图-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-13 12:24:26
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 正文
虽然从技术上来说AJAX应用确实是单页面应用（即在第一个请求时加载一个HTML页面，后面只刷新DOM中的局部区域），但是很多时候，出于各种原因，我们需要为用户展示或者隐藏一些子页面视图。

我们可以利用Angular的$route服务来管理这种场景。你可以利用路由服务来定义这样一种东西：对于浏览器所指向的特定URL，Angular将会加载并显示一个模板，并实例化一个控制器来为模板提供内容。

在应用中，你可以通过调用$routeProvider服务上的函数来创建路由，把需要创建的路由当成一个配置块传递给这些函数即可。创建过程类似下面的伪代码：

```
var someModule = angular.module('someModule', [... module dependencies ...]);

someModule.config(function ($routeProvider){
	when('url', {controller: aController, templateUrl: '/path/to/tempate'}).
	when(... other mappings for your app ...).

	...

	otherwise(... what to do if nothing else matches ...);
})
```

以上代码是说，当浏览器中的URL变成指定的取值时，Angular将会加载/path/to/tempate路径下的模板，然后把这个模板中的根元素关联到aController上（因为我们在模板中写了ng-controller=aController）。

最后一行中的otherwise()调用可以告诉路由，在没有配置到任何东西时跳转到哪里。

我们来把这一技术投入应用，构建一款email应用，这款应用将会轻松超越Gmail、Hotmail，以及一切mail，我们把它叫做AMail。现在我们从最简单的功能入手。我们会有一个首页视图，里面将会展示邮件列表，其中带有日期、标题和发件人。当点击一份邮件时，将会打开一个新视图来展示邮件内容。

如果你想自己尝试运行以上代码，由于浏览器由安全限制，因此需要把代码放到web服务器中，而不是仅仅使用file://方式来访问。如果你安装了Python，就可以在工作目录来执行python -m SimpleHTTPServer 8888来启动服务器。

在主模板中，我们会做一些不同的东西。我们不会把所有东西都放在第一个加载的页面中，而是仅仅创建一个布局模板，然后再用这个模板来容纳各种视图。在主视图中，我们想回把多个视图共有的东西都放在里面，例如菜单。在这个例子中，我们仅仅把应用的标题放在里面，然后再用ng-view指令来告诉Angular把视图显示在哪儿。

### index.html

```
<!DOCTYPE html>
<html lang="en" ng-app="AMail">
<head>
	<meta charset="UTF-8">
	<title>AMail</title>
	<script src="../angular.js"></script>
	<script src="controller.js"></script>
</head>
<body>
	<h1>A-Mail</h1>
	<div ng-view></div>
</body>
</html>
```

由于所有视图模板都会被插入刚才所创建的外壳中，所以可以把它们都编写成HTML文档片段。对于邮件列表视图而言，我们将会使用ng-repeat指令来遍历邮件列表，然后把它们渲染到一个table中。

### list.html
```
<table>
	<tr>
		<td><b>Sender</b></td>
		<td><b>Subject</b></td>
		<td><b>Date</b></td>
	</tr>
	<tr ng-repeat="message in messages">
		<td>{{message.sender}}</td>
		<td><a href="#/view/{{message.id}}">{{message.subject}}</a></td>
		<td>{{message.date}}</td>
	</tr>
</table>
```

注意这里，我们想要实现用户点击一个主题就能被导航到相应的邮件中。我们已经在URL和message.id之间进行了数据绑定，所以用户点击id=1的邮件就会被导航到#/view/1。<b>这种根据URL导航的方式也叫作深度链接</b>，我们将会在邮件详细视图控制器中使用这种方法，从而实现把一份可用的邮件链接到对应的详细视图上的功能。

为了创建这个邮件详情视图，需要创建一个模板，用来展示单个邮件对象上的属性。

### detail.html
```
<div><b>Subject:</b>{{message.subject}}</div>
<div><b>Sender:</b>{{message.sender}}</div>
<div><b>Date:</b>{{message.date}}</div>

<div>
	<b>To:</b>
	<span ng-repeat="recipient in recipients">{{recipient}}</span>
	<div>{{message.message}}</div>
	<a href="#/">Back to message list</a>
</div>
```

为了把这些模板关联到对应控制器上，我们将会给$routeProvider配置一个URL，$routeProvider将会负责调用控制器和模板。

### controller.js
```
//为核心的AMail服务创建模块
var aMailServices = angular.module('AMail', []);

//在URL、模板和控制器之间建立映射关系
function emailRouteConfig($routeProvider){
	$routeProvider.
	when('/', {
		controller: ListController,
		templateUrl: 'list.html'
	}).

	//注意，为了创建详情视图，我们在id前面加了一个冒号，从而指定了一个参数化的URL组件
	when('/view/:id', {
		controller: DetailController,
		templateUrl: 'detail.html'
	}).

	otherwise({
		redirectTo: '/'
	})
};

//配置我们的路由，以便AMail服务能够找到它
aMailServices.config(emailRouteConfig);

//一些虚拟的邮件
messages = [
	{
		id: 0,
		sender: '000@sender.com',
		subject: '第一封邮件',
		date: '2016-07-01',
		recipients: ['000@recipients.com'],
		message: 'Hello Angular！'
	},
	{
		id: 1,
		sender: '111@sender.com',
		subject: '第一封邮件',
		date: '2016-07-01',
		recipients: ['111@recipients.com'],
		message: 'Hello Angular！'
	},
	{
		id: 2,
		sender: '222@sender.com',
		subject: '第一封邮件',
		date: '2016-07-01',
		recipients: ['222@recipients.com'],
		message: 'Hello Angular！'
	},
	{
		id: 3,
		sender: '333@sender.com',
		subject: '第一封邮件',
		date: '2016-07-01',
		recipients: ['333@recipients.com'],
		message: 'Hello Angular！'
	}
];

//把我们的邮件发布给邮件列表模板
function ListController($scope){
	$scope.message = messages;
}

//从路由信息（从URL中解析出来的）中获取邮件id，然后用它找到正确的邮件对象
function DetailController($scope, $routeParams){
	$scope.message = messages[$routeParams.id];
}
```

我们已经为应用搭好了基本的框架，这款应用带有很多视图，可以通过修改URL的方式来切换视图。这意味着对用户来说前进和后退按钮能够真正运行起来了。用户可以把应用内部的视图链接加入书签或者通过邮件发送出去，而实际上这里只有唯一一个真实的HTML页面。
