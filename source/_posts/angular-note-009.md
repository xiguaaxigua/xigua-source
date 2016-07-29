---
title: 009-隐藏和显示-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-09 21:53:39
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

### 隐藏和显示

对于菜单、上下文敏感的工具以及很多情况来说，显示和隐藏元素是一项核心的功能。与Angular中其他功能一样，我们通过修改数据模型的方式来驱动UI刷新，然后通过指令会把变更反应到UI上。

这个例子将会使用ng-show和ng-hide。这两条指令的功能是等价的，但是运行效果正好相反，它们都可以根据你所传递的表达式来显示或者隐藏元素。也就是说，ng-show在表达式为true时将会显示元素，为false时将会隐藏元素；而ng-show哪一条指令更能表达你的意图，你就使用哪一条。

这两条指令的工作原理是：根据实际情况把元素的样式设置为display:block来显示元素；设置为display:none来隐藏元素。我们来看一个虚构的例子，在这个例子中我们将为Death Ray构建控制面板。

``` html
<div ng-controller="DeathrayMenuController">
	<button ng-click="toggleMenu()">Toogle Menu</button>
	<ul ng-show="menuState.show">
		<li ng-click="stun()">Stun</li>
		<li ng-click="disintegrate()">Disintegrate</li>
		<li ng-click="erase()">Erase from history</li>
	</ul>
</div>
<script>
	function DeathrayMenuController($scope){
		$scope.menuState.show = false;

		$scope.toggleMenu = function(){
			$scope.menuState.show = !$scope.menuState.show;
		};

		//Death Ray的具体功能留给读者自行练习
	}
</script>
```
