---
title: 010-CSS类和样式-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-11 08:42:59
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

### CSS类和样式
现在很明显，你可以在你的应用中动态的设置CSS类和样式了，只要使用双花括号插值语法把他们进行数据绑定即可。你甚至还可以在模板中构造CSS类名的部分匹配方式。例如，如果你想要有条件地禁用某些菜单，你可能会做以下事情来给用户视觉是的提示。

假设有下面这样一段CSS：

```
.menu-disabled-true{
	color: grey;
}
```

使用一下模板，你就可以把Death Ray的stun（击晕）功能显示成禁用状态：

```
<div ng-controller="DeathrayMenuController">
	<ul>
		<li class="menu-disabled-{{isDisabled}}" ng-click="stun()">Stun</li>
		...
	</ul>
</div>
```

然后你就可以根据需要，通过控制器来设置isDisabled属性了：

```
function DeathrayMenuController($scope){
	$scope.isDisabled = false;

	$scope.stun = function(){
		//击晕目标，然后禁用复活按钮
		$scope.isDisabled = true;
	}
}
```

Stun菜单项上的CSS类将会被设置为menu-disabled-加上$scope.isDisabled的值。 

### 反思src和href属性

当在img或者a标签上进行数据绑定时，在src或者href属性上简单使用双花括号插值语法无法很好地运行。由于浏览器会优先使用并行的方式来加载图片和其他内容，所以Angular没有机会拦截到数据绑定请求。img标签最自然地用法是：

```
<img src="/images/cats/{{favoriteCat}}">
```

但其实，这里应该使用ng-src指令，你的模板应该写成这样：

```
<img ng-src="/images/cats/{{favoriteCat}}">
```

类似地，对于a标签，应该使用ng-href指令：

```
<a ng-href="/shop/category={{numberOfBallons}}">Some Text</a>
```
