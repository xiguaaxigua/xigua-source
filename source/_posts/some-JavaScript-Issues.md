---
title: 几个关于JavaScript的问题
date: 2016-07-04 12:47:23
tags: [JavaScript]
---
严格模式；var a=b=1；自执行函数；a和window.a。
<!--more-->

今日，在[豪情哥](https://www.zhihu.com/people/jikey)的前端交流群（群号：570259839）里遇到几个很有（wo）意（bu）思（hui）的问题，感谢[精灵哥](https://github.com/hstarorg/HstarDoc)的帮助。

现记录如下。

欢迎扫扫二维码同大家一起交流，该群大手子太多，慎加（嘿嘿嘿）：

<img src="http://7xtoaz.com1.z0.glb.clouddn.com/HaoQing-QQ-Group-QR-Code.jpg" style="width:30%"/>

### 问题1

以下代码的运行结果？
```
(function(){
    var a = b = 1;
})();
console.log(b);
```
运行结果是：1。`(function(){})();`自执行函数，函数体的内部声明了b，且b为全局变量。相当于：
```
(function(){
    window.b = 1;//声明全局变量b
    var a = window.b;//声明局部变量a
})();
```

### 问题2
以下代码的运行结果？
```
(function(){
    'use strict';
    var a = b = 1;
})();	
console.log(b);
```
运行结果是：报错，严格模式下变量必须通过`var`关键字声明，即不允许隐式声明变量。

### 问题3
以下代码的运行结果？
```
(function(){
    'use strict';
    var a = window.b = 1;
})();
console.log(b);
```
运行结果是：1。严格模式下，访问全局变量可以不通过`window`对象。相当于：
```
(function(){
    'use strict';
    window.b = 1;//全局变量b
    var a = b;
})();
console.log(b);
```

`a`和`window.a`的区别：
+ `a`是在当前作用域查找，如果当前作用域没有，则继续往上找，直到找到为止。
+　`window`是顶级作用域。

### 问题4
以下代码的运行结果？
```
var a = 2;
var func = (function(){
    var a = 3;
    return function(){
        a++;
        alert(a);
    }
})();
console.log(func);
func();
func();
```
运行结果是：4，5。


### 问题5
以下代码的运行结果？
```
var a = 2;
var func = (function(){
    var a = 3;
    return function(){
        this.a++;//window.a++
        alert(a);//弹出的是局部变量a，而不是window.a
    }
})();
console.log(func);
func();
func();
```
运行结果是：3，3。

### 总结
+ 函数的作用域是定义时的作用域，和谁执行无关。
+ 函数的this，和在哪儿定义无关，和谁执行相关。谁执行谁就是this。

### 相关页面
+ [window.a和一个在window环境中的a不一样吗](https://segmentfault.com/q/1010000004676348/a-1020000004677057)
+ [IE全局变量的Dissociative Identity Disorder](http://hax.iteye.com/blog/349569)
