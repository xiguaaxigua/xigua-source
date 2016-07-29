---
title: SeaJS笔记
date: 2016-07-18 19:17:30
tags: [JavaScript, 框架]
---

A是正房，C是小三。
<!-- more -->

![](http://7xtoaz.com1.z0.glb.clouddn.com/seajs.jpg)
## SeaJS

### 常用API
经常使用的API有：define、require、require.async、exports、module.exports等。

#### define(factory)
`define` 是一个全局函数，用来定义模块。 `factory` 可以是一个函数，也可以是一个对象或字符串。

`factory` 为对象、字符串时，表示模块的接口就是该对象、字符串。比如可以如下定义一个 `JSON` 数据模块：
```
define({ "foo": "bar" });
```

也可以通过字符串定义模板模块：
```
define('I am a template. My name is {{name}}.');
```

`factory` 为函数时，表示是模块的构造方法。执行该构造方法，可以得到模块向外提供的接口。`factory` 方法在执行时，默认会传入三个参数：`require`、`exports` 和 `module`：
```
define(function(require, exports, module) {

  // 模块代码

});
```

#### require(id)
require是一个方法，接受模块标识作为唯一参数，用来获取其他模块提供的接口。
```
define(function(require, exports, module)(
	
	// 获取a模块的接口
	var a = require('./a');

	// 调用a模块的方法
	a.doSomething();
));
```

在SeaJS中使用require时，需要遵循一些约定：
+ factory的第一个参数必须为require，且不能缩写；
+ 不允许覆盖require函数；
+ require的参数必须为字符串直接量。

#### require.async(id, [callback])
用来在模块内部异步加载另一个模块，并且在加载成功时执行callback回调函数。

和require的区别：
+ require是同步往下执行。
+ require.async是异步回调执行，一般用来加载可延迟异步加载的模块。

#### export
export是一个对象，用来向外提供模块接口。
```
define(function(require, exports, module){
	
	function welcome(name){
		this.name = name;
	}

	welcome.prototype.sayHello = function(){
		console.log('Hello ' + this.name);
	}

	module.exports = welcome;
});
```

除了使用exports向外提供接口，还可以使用return：
```
define(function(require, exports, module){
	
	// 通过return向外提供接口
	return {
		name: 'World',
		sayHello: function(){
			console.log('Hello ' + this.name);
		};
	}
});
```

如果return语句是模块中的唯一代码，还可以更简单：
```
// 这种格式很适合用来定义JSONP模块
define({
	name: 'World',
	sayHello: function(){
		console.log('Hello ' + this.name);
	}
})
```

#### module
module是一个对象，上面存储了与当前模块相关联的一些属性和方法。

#### module.exports
当前模块向外提供的接口。
