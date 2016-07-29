---
title: 写插件时遇到的疑惑
date: 2016-07-16 22:05:26
tags: [JavaScript, 框架]
---

$.fn、$.extend和$.fn.extend这三个概念在jQuery和Zepto中的定义竟然是不一样的。
<!--more-->
新手伤不起，为了工作，好多概念都是会用就行，如今恍然大悟，底层的概念都搞不明白谈何高楼大厦啊！

## jQuery
### $.fn在jQuery中的定义
jQuery是怎么定义$.fn的：
```
jQuery.fn = jQuery.prototype ={ 
　　　
}
```
可以看到，$.fn指向jQuery的prototype。因此，$.fn上的方法和属性可以分为三类：
+ jQuery对象的方法和属性；
+ $.fn自己的方法和属性；
+ 你自己扩展到$.fn的方法和属性。

每个jQuery实例都可以调用$.fn上的方法和属性。

Demo：
```
<h1 id="demo">Hello World</h1>
<script src="jquery.min.js"></script>
<script>
	;(function(){

		$.fn.toRed = function(){
			$(this).css('color', 'red');
		};
	})(jQuery);

	// 调用刚才定义的toRed()方法
	$('#demo').toRed(); // 运行结果：h1标签内的文本变为红色
</script>
```

### $.extend在jQuery中的定义
`$.extend(Object)` ，可以用来扩展jQuery对象本身，自定义的方法和对象将扩展到jQuery本身。可以在引入jQuery的地方调用自定义的方法。

```
<script>
	;(function(){

		$.extend({
			add: function(a, b){
				return a+b;
			}
		});
	})(jQuery);

	console.log($.add(10, 11)); // 21
</script>
```

### $.fn.extend在jQuery中的定义
`$.fn.extend(object)` ，用来扩展 `jQuery.prototype`，任何jQuery的实例都可以调用自定义的方法。
```
<h1 id="demo">Hello World</h1>
<script src="jquery.min.js"></script>
<script>
	;(function(){

		$.fn.extend({
                toYellow: function(){
                    $(this).css('color', 'yellow');
                }
            });
	})(jQuery);

	// 调用刚才定义的toYellow()方法
	$('#demo').toYellow(); // 运行结果：h1标签内的文本变为黄色
</script>
```

那么问题来了。
+ 貌似 `$.fn` 和 `$.fn.extend()` 可以达到同样的效果，那么两者的区别是什么？
+ 静态方法和成员方法是什么？

## Zepto
### $.fn在Zepto中的定义
这里貌似和jQuery中的$.fn是一样的，$.fn拥有Zepto的所有方法，和其它方法，在这个对象上添加一个方法，所有Zepto的实例都可以用到。

```
<h1 id="demo">Hello World</h1>
<script src="zepto.min.js"></script>
<script>
    
    ;(function(){

        $.fn.toRed = function(){
            $(this).css('color', 'red');
        };
    })(Zepto);

    $('#demo').toRed();
</script>
```

### $.extend在Zepto中的定义
试着以刚才jQuery中$.extend的方式扩展Zepto对象本身，发现报错了！
```
;(function(){

    $.extend({

        add: function(a, b){
            return a+b;
        }
    });
})(Zepto);

$.add(10, 11); // $.add is not a function
```

然后Zepto的源码发现，在Zepto中是这样定义$.extend的：
```
// Copy all but undefined properties from one or more
// objects to the `target` object.
$.extend = function(target){
	var deep, args = slice.call(arguments, 1)
	if (typeof target == 'boolean') {
	  deep = target
	  target = args.shift()
	}
	args.forEach(function(arg){ extend(target, arg, deep) })
	return target
}
```
通过源对象扩展目标对象的属性，源对象属性将覆盖目标对象。默认情况下是浅拷贝，如果第一个参数为true为深拷贝。

试着写个Demo：
```
var superMan = {skill: '生孩子'};
var man = {sex: '男人', skill: '打手枪'};

$.extend(man, superMan);
console.log(man); // Object {sex: "男人", skill: "生孩子"}
```
好像Zepto里的$.extend只是用来扩展对象的啊。

扩展一下Zepto本身试试：
```
var superMan = {skill: '生孩子'};
$.extend(Zepto, superMan);
console.log(Zepto.skill); // 生孩子
```
好吧，貌似是姿势不对。jQuery用以上姿势又当如何呢？

```
<script src="http://xiguaaxigua.cn/js/jquery.min.js"></script>
<script>
    
    ;(function(){

        var superMan = {skill: '生孩子'};
        var man = {sex: '男人', skill: '打手枪'};

        $.extend(man, superMan);
        console.log(man); // Object {sex: "男人", skill: "生孩子"}
    })(jQuery);
</script>
```
好像也是可以的，再试一下：
```
<script src="http://xiguaaxigua.cn/js/jquery.min.js"></script>
<script>
    
    ;(function(){

        var superMan = {skill: '生孩子'};
        var man = {sex: '男人', skill: '打手枪'};

        $.extend(jQuery, superMan);
        console.log(jQuery.skill); // 生孩子
    })(jQuery);
</script>
```
jQuery和Zepto中的$.extend都可以用来扩展目标对象。不同的是传入的参数不一样。
+ 都可以传入目标对象和源对象，达到扩展目标对象的目的。
+ jQuery中的$.extend可以接受自定义的方法，扩展jQuery对象本身。

此处有疑问：
+ 什么是浅拷贝和深拷贝？

### $.fn.extend在Zepto中的定义
试着以jQuery的形式跑一遍，发现报错了，
```
<h1 id="demo">Hello World</h1>
<script src="zepto.min.js"></script>
<script>
	;(function(){

		$.fn.extend({
                toYellow: function(){
                    $(this).css('color', 'yellow');
                }
            });
	})(Zepto);

	$('#demo').toYellow(); // $.fn.extend is not a function
</script>
```

额，是不是Zepto没有这个方法？
```
console.log($.fn.extend()); // $.fn.extend is not a function
```

好吧，好像还真没有。

## 答疑解惑
感谢[@精灵哥](http://www.cnblogs.com/humin/)的帮助。

为了记录一下思考的过程，就不在原文里更改了。

### 什么是对象方法？什么是类方法？什么是原型方法？
```
function Man(name){
	this.name = name;

	// 对象方法
	this.sayHello = function(){
		console.log('Hello ' + this.name);
	}
}

// 类方法
Man.sayFuck = function(){
	console.log('Fuck ' + this.name);
}

// 原型方法
Man.prototype.sayWhat = function(){
	console.log('What? ' + this.name);
}

var m1 = new Man('老王');

m1.sayHello(); // Hello 老王
Man.sayHello(); // Man.sayHello is not a function

m1.sayFuck(); // m1.sayFuck is not a function
Man.sayFuck(); // Fuck Man

m1.sayWhat(); // What? 老王
Man.sayWhat(); // Man.sayWhat is not a function
```
从这三个方法的调用可以看出三者的区别：
+ Man的实例m1可以访问Man的对象方法。
+ 不需要实例Man就可以访问Man的类方法。
+ 原型方法只有实例可以调用，常用于共享方法。

### 什么是实例方法？什么是静态方法？
```
class Man{
	constructor(){
		this.func1 = function(){
			console.log('我是实例方法');
		}
	}
	static func2(){
		console.log('我是静态方法');
	}
}
var m1 = new Man();

// 实例方法只能Man的实例调用
Man.func1(); // Man.func1 is not a function
m1.func1(); // 我是实例方法

// 静态方法只能Man自己调用
Man.func2(); // 我是静态方法
m1.func2(); // m1.func2 is not a function
```
+ 实例方法是实例化对象的时候，就会产生一个实例方法，即当前实例可以调用对象的所有实例方法。
+ 静态方法是挂载在对象本身上的，只有该对象可以调用。

### $.fn和$.fn.extend的区别？
```
$.fn.extend = function(name, func){
	$.fn[name] = func;
}
```
这两种方法确实是一样的，只是一个是方法形式。

### 浅拷贝和深拷贝？
<b>浅拷贝复制引用，深拷贝创建实例。</b>
浅拷贝：简单的赋值就是浅拷贝，因为对象和数组在赋值的时候都是引用传递。
```
var a = [1, 2, 3];
var b = a;
console.log(b); // [1, 2, 3]
```

深拷贝：
```
var a = [1, 2, 3];
var deepCopy = [];

function demo(a1, a2){
    for (var i=0; i<a1.length; i++){
        a2[i] = a1[i];
    }
}

demo(a, deepCopy);
console.log(a); // [1, 2, 3]
console.log(deepCopy); // [1, 2, 3]

deepCopy[0] = 100;
console.log(deepCopy); // [100, 2, 3]
```

## End
That`s aLL.
