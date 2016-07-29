---
title: 013-利用$scope暴露模型数据-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-12 11:13:52
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

### 利用$scope暴露模型数据

利用向控制器传递$scope对象的机制，可以把模型数据暴露给视图。在你的应用中可能还有其他数据，但是只有通过$scope触及这些数据，Angular才会把它当成数据模型的一部分。你可以把$scope当成一个上下文环境，它让数据模型上的变化变得可以观察。对于显式地创建$scope属性我们已经看到过很多例子了，例如$scope.count = 5。还可以间接地通过模板自身创建数据模型。你可以通过以下几种方式来实现这一点。

1.通过表达式。

既然表达式是在控制器的$scope环境中执行的，而这个$scope与它们所管理的元素有关，那么在表达式中设置属性值就和设置控制器的$scope属性值是一样的。也就是说，这样做：

```
<button ng-click="count=3">Set Count To Three</button>
```

与下面这种做法的效果相同：

```
<div ng-controller="CountController">
	<button ng-click="setCount()">Set Count To Three</button>
</div>
```

CountController定义如下：

```
function CountController($scope){
	$scope.setCount = function(){
		$scope.count = 3;
	}
}
```

2.在表单输入项上使用ng-model。

与表达式类似，ng-model上指定的模型参数同样工作在外层控制器内。唯一的不同点在于，这样会在表单项和指定的模型之间建立双向绑定关系。
