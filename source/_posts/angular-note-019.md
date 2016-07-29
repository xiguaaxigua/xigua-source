---
title: 019-与服务器交互-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-14 13:29:07
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 正文
真正的应用需要和真实的服务器进行交互，移动应用和新兴的Chrome桌面应用可能是个例外，但是对于此外的所有应用来说，无论你是想把谁持久化到云端，还是需要与其他用户进行实时交互，都需要让应用与服务器进行交互。

为了实现这一点，Angular提供了一个叫做$http的服务。它提供了一个可扩展的抽象方法列表，使得与服务器的交互更加容易。它支持HTTP、JSONP和CORS方式。它还包含了安全性支持，避免JSON格式的脆弱性和XSRF（跨域请求伪造）。它让你可以轻松地转换请求和响应数据，甚至还实现了简单的缓存。

例如，我们打算让购物站点从服务器上获取商品信息，而不是从内存读取假数据。如何编写服务端代码已经超越了本书的范畴，所以我们仅仅来想想一下，比方说我们已经创建了一个服务器，放查询/products这个路径时，它会以JSON格式返回一个商品列表。

返回的响应示例如下：

```
[
	{
		"id": 0,
		"title": 'Title 0',
		"description": 'Description 0',
		"price": 1.0
	},
	{
		"id": 1,
		"title": 'Title 1',
		"description": 'Description 1',
		"price": 1.1
	},
	{
		"id": 2,
		"title": 'Title 2',
		"description": 'Description 2',
		"price": 1.2
	}
	... etc ...
]
```

我们可以像下面这样编写查询代码：

```
function ShoppingController($scope, $http){
	$http.get('/products').success(function(data, status, headers, config){
		$scope.items = data;
	})
}
```

然后在模板中这样使用它：

```
<body ng-controller="ShoppingController">
	<h1>Shop!</h1>
	<table>
		<tr ng-repeat="item in items">
			<td>{{item.title}}</td>
			<td>{{item.description}}</td>
			<td>{{item.price | currency}}</td>
		</tr>
	</table>
</body>
```

正如我们前面讲过的，从长远来看，让服务来代理与服务器交互的工作对我们有好处，这个服务可以被多个控制器共享。
