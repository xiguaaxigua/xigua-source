---
title: 依赖注入
date: 2016-05-03 11:01:04
tags: [JavaScript, Angular]
---
初涉设计模式之依赖注入。
<!--more-->

## 什么是依赖注入
### 依赖注入是一种模式
首先，依赖注入是一种设计模式，因为他解决的是一类问题。

<b>[什么是设计模式?](http://baike.baidu.com/link?url=SFiSGjteeq27Y6W33cqx1Mai0YGjmuiWomdhiuyD_cZPc4MsbIFfcvErfV6JQMPczMiIjIF6eljxKBJ0vxP8Uq)</b>
设计模式（Design pattern）是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。 毫无疑问，设计模式于己于他人于系统都是多赢的；设计模式使代码编制真正工程化；设计模式是软件工程的基石脉络，如同大厦的结构一样。

### 依赖注入的作用域
要知道依赖注入是解决什么问题，最好先了解一个原则：
<b>依赖倒转原则(Dependence Inversion Priciple, DIP)</b>提倡：
+ 高层模块不应该依赖低层模块。两个都应该依赖抽象
+ 抽象不应该依赖细节，细节应该依赖抽象
+ 针对接口编程，不要针对实现编程

在编程时，我们对系统进行模块化，它们之间有依赖，比如模块A依赖模块B，那么依据DIP，模块A应该依赖模块B的接口，而不应该依赖模块B的实现，这样做的好处就不详叙了。

下图描述了这个关系图：
![](http://7tszky.com1.z0.glb.clouddn.com/Fhe152elkbDvQo8SPbpByySnszgN)

这里需要注意一点，虽然模块A只依赖接口编程，但在运行的时候，它还是需要有一个具体的模块来负责模块A需要的功能的，所以模块A在【运行时】是需要一个【真的】模块B，而不是它的接口。所以上图中，Module和Interface之间的线是包含，而不是关联。也就是说，模块A在【运行时】需要有一个接口的实现模块作为它的属性，那么这个实现模块怎么来？它是怎么初始化，然后怎么传给模块A的？

解决这个问题的就是依赖注入，这就是它的作用域。

<i>上面的结构图再扩展一下就是非常著名的设计模式——桥接</i>

### 前端的依赖注入
对于前端来说，很少有抽象，更别说有接口了，但是，依赖注入却是一直都存在，只是许多同学没有认出来而已，下面来看看前端最常见的一个依赖注入：
``` javascript
// moduleA.js
define('moduleA', ['moduleB'], function(moduleB) {
    return {
        init: function() {
            this.I_need = ModuleB.someFun();
        }
    };
});
```
这是个很普通的代码，太正常了，我们每天都会写这些代码，即使define包裹可能是构建帮我们写的，还记得前面说的依赖注入的作用域，它只做两件事：
+ 初始化被依赖的模块
+ 注入到依赖模块中

这个时候应该知道了，define就是做这些事的：
+ 它负责初始化moduleB
+ 它通过函数参数的形式注入到moduleA里面去

## 依赖注入的作用
为什么需要依赖注入？它的作用和意义是什么？
关于这个，我们还是要从依赖注入做了什么事来探索：
+ 1. 初始化被依赖的模块
如果不通过依赖注入模式来初始化被依赖的模块，那么就要依赖模块自己去初始化了
那么问题来了：依赖模块就耦合了被依赖模块的初始化信息了
+ 2. 注入到依赖模块中
被依赖模块已经被其他管理器初始化了，那么依赖模块要怎么获取这个模块呢？
有两种方式：
 + 自己去问
 + 别人主动给你

没用依赖注入模式的话是1，用了之后就是2，想想，你需要某个东西的时候，你去找别人要，你需要提供别人什么信息？最简单的就是那个东西叫什么，是的，正式一点，你需要一个名称，没错，方式1的问题是：依赖模块耦合了被依赖模块的【名称】还有那个【别人】，而方式2解决了这个问题，让依赖模块只依赖需要的模块的接口。

<i>可以看到，注入的两个方式的主动权是相反的。因此，依赖注入(Dependency Injection, DI)有时候也被称为 控制反转(Inversion of Control, IoC)，它们不是一个东西，有兴趣的同学可以深入学习</i>

### 代码解释
文字比较抽象，那么我们用代码来说明依赖注入的作用以及好处
``` javascript
// config.js
require.config = {
    path: {
        jquery: 'common/jquery'
    }
};

// moduleA.js
define('moduleA', ['jquery'], function($) {
    return {
        init: function() {
            this.$dom = $('#id');
        }
    };
});
```
用过模块加载器的都知道，一般我们可以配置怎样去获取模块的定义，也就是模块的实现代码
一般是通过配置文件的形式
上面的代码很简单，moduleA依赖了jquery库，在模块加载器中，我们配置了jquery模块在哪里初始化
可以看到，jquery模块的代码是在本地的
现在，不管什么原因，我们想要使用一个线上代码库版本的jquery，怎么办？简单：
``` javascript
// config.js
require.config = {
    path: {
        jquery: 'http://path/to/online/jquery'
    }
};
```
可以看到，我们只需要修改模块加载器的配置就可以了
这个配置就是被依赖模块（jquery）的初始化信息
这个就是依赖注入的第一个好处：<b>依赖模块与被依赖模块的初始化信息解耦</b>
这个例子也是很常见的代码：
``` javascript
// moduleA.js
var $ = require('jquery');

module.exports = {
    init: function() {
        this.$dom = $('#id');
    }
};
```
聪明的同学已经看到问题在哪里了，没错，这个模块依赖了被依赖模块的名字
这里会有两个问题：
+ 模块重名问题，还记得那些年我们给模块起名字的日子吗？
+ 改变模块依赖方式
像jquery这种库，有许多都是最先加载，并且全局使用的：
``` javascript
// moduleA.js
module.exports = {
    init: function() {
        this.$dom = $('#id');
    }
};
```
对于这种情况，我们的组件代码就得改动了
不同的模块依赖方式给通用组件的实现造成了很大的困扰
为了不改动组件代码，通常我们这样做：
``` javascript
// jquery.js
module.exports = window.$;
```
当然，这是题外话了
从上面的例子应该可以知道，依赖注入帮助我们解决了依赖模块对被依赖模块的初始化解耦

## 依赖注入模式的实现细节
### 组件容器（模块管理器）
一般依赖注入模式都实现在某个容器中，在前端我们可以管它为模块管理器
组件容器负责管理所有的组件，管理他们的初始化，以及依赖，并提供接口获取组件
通常容器会把组件的初始化信息聚集在某个配置文件中，比如xml文件或者json文件等
这样做的好处是可以很轻易的修改组件的初始化信息，并且可以实现组件的热启动
对于前端来说，模块管理器，比如requireJs，就是负责模块的初始化工作的
但是模块加载器的重心不是依赖注入
因此这里提供一个依赖注入容器的简单例子：
``` javascript
// injector
// APP Instance -- Global & Singleton
var injector = {
    set: function(name, factory) {
        // name: the dependency name
        // factory: can be a factory function
        //          or just a value
    },
    get: function(name) {}
};

// a.js
injector.set('env', 'dev');

// b.js
injector.set('b', function() {
    return {
        sayYes: function() {
            console.log('Yes!');
        },
        sayNo: function() {
            console.log('No!');
        }
    };
});

// c.js
injector.set('c', function(env, b) {
    if (env === 'dev') {
        b.sayYes();
    } else {
        b.sayNo();
    }
});
```
实现起来并没有难点，injector其实就只是个map
用factory函数的好处是可以延迟模块的初始化
另外一个难点是要读取函数的形参名，但是我们也可以这样改来避开这个难点：
``` javascript
// injector
var injector = {
    set: function(name, array) {
        // name: the dependency name
    },
    get: function(name) {}
};

// c.js
injector.set('c', ['env', 'b', function(env, b) {
    if (env === 'dev') {
        b.sayYes();
    } else {
        b.sayNo();
    }
}]);
```

### 初始化
可以看到模块管理器实际上只是一个容器
现在我们需要一个初始化模块，下面提供一个小栗子：
``` javascript
// initializer.js
function initializer() {
    // to load the module in initializer.config
}

initializer.config = {
    initList: ['./a.js', './b.js', 'http://path/to/other/module.js'],
    map: {
        'jquery': 'http://path/to/online/jquery.js'
    }
};
```
initializer();
可以看到，如果文件内容本身就有注册模块的代码的话，initializer只需要加载js文件即可，比如上面的a.js和b.js文件
当然也可以加载线上资源
如果文件内容没有注册模块的代码的话，就需要initializer自己帮忙注册了
比如栗子中的jquery
如果系统是服务器端的nodejs代码的话，就可以实现模块的热插拔了

### 注入方式
被依赖模块怎样赋值给依赖模块，主要有三种方式
####构造函数注入
前面define和angular的依赖注入都是使用构造函数的注入方式，如下：
``` javascript
// define
define('moduleA', ['moduleB'], function(moduleB) {
    return {
        init: function() {
            this.I_need = ModuleB.someFun();
        }
    };
});

// anguler
someModule.controller('MyController', ['$scope', 'greeter', function($scope, greeter) {
  // ...
}]);
```

#### setter注入
直接上例子：
``` javascript
// moduleA.js
var moduleA = {
    do: function() {
        this.helper.doSomething();
    },
    setHelper: function(helper) {
        this.helper = helper;
    }
};

// initializer.js
function initializer() {
    // ...
    moduleA.setHelper(new Helper());
}
```

#### 接口注入
接口注入主要是把注入过程抽象成接口的形式，让注入方式可以被轻易扩展
在前端并不怎么使用接口，因此这种注入方式就不详述

## 对比——服务定位模式 (Service Locator, SL)
读者可能对服务定位模式不太了解，但是看了下面的代码就知道了
``` javascript
var fs = require('fs');
var path = require('path');
var moduleB = require('./moduleB');
var moduleC = require('path/to/moduleC');
```
没错，require就是一个服务定位模式
所谓的服务定位模式就是把所有服务（模块）资源的管理都放到一个定位者那里
所有需要服务的模块都找它要就行了，就是这么简单
服务定位模式也能解决依赖注入的作用域问题
服务定位者负责初始化服务，它也提供服务资源
只是依赖注入是被动，服务定位模式需要模块自己主动去请求，详见【3. 依赖注入的作用】
对于前端来说，
服务定位模式肯定更常见，它的优点就是简单，缺点是所有模块都需要依赖定位者
依赖注入模式的优点是控制反转，更利于组件化，缺点是不是前端的基础能力（谁让require是基础。。。）

## 结语
依赖注入模式并不神秘，也不是什么高大上
Java时代的Spring就已经把依赖注入推向顶峰
本文只想向前端同学传达：依赖注入的思想非常值得学习

<p style="text-align: right;">[原文地址](http://imweb.io/topic/571b567505637d4c67ae3f64)</p>