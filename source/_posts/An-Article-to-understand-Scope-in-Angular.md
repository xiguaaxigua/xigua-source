---
title: 一篇文章搞懂Angular中的Scope
date: 2016-05-25 11:16:24
tags: [Angular, JavaScript]
---
Angular中的作用域。
<!--more-->

近日在琢磨Angular中的Scope时，有些着迷，![](http://7xtoaz.com1.z0.glb.clouddn.com/mengbi.jpg)进而发现JavsScript果然博（shi）大（fen）精（cao）深（dan），原本对js原型继承一知半解的博主更加Mengbility。
![](http://7xtoaz.com1.z0.glb.clouddn.com/mengbi2.jpg)

好在一上午鏖战以后“三脸懵逼”的博主略有进步，现记录如下：

## 正文
先来举个栗子，让我们来谈谈老王和小王的故事，这是一个精（wei）彩（suo）的故事，也是下面代码想要实现的效果！下面是上代码：

<i>故事讲的是小王继（xin）承（shang）老王的小电影的故事，让我们来约定Avideo（某一部你懂得）指代JavsScript中的基本数据类型（String、Number、Boolean、Null、Undefined），cloudObj（云盘是个对象，可以存好多东西你又懂了是不是）指代JavsScript中的复合数据类型（Object），这对理解本文很重要，因为到处都是av的栗子！</i>

### 目录结构：
——index.html
——app.js

### index.html的代码
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Scope in Angular</title>
    <script src="https://cdn.bootcss.com/angular.js/1.5.5/angular.min.js"></script>
    <script src="app.js"></script>
    <style>
        .laowang{
            border: 2px solid #000;
        }
        .xiaowang{
            border: 1px solid #green;
            margin: 2px;
        }
    </style>
</head>
<body>
<div ng-app="myApp">
    <div class="laowang" ng-controller="LaowangCtrl">
        <h1>{{Avideo}}</h1>
        <h1>{{cloudObj.Avideo}}</h1>

        <div class="xiaowang" ng-controller="XiaowangCtrl">
            <h3>{{Avideo}}</h3>
            <h3>{{cloudObj.Avideo}}</h3>
        </div>
    </div>
</div>
</body>
</html>
```

### app.js的代码

```
var app = angular.module('myApp', []);

app.controller('LaowangCtrl', function($scope){

    $scope.Avideo = "老王的AV";
    $scope.cloudObj = {};
    $scope.cloudObj.Avideo = "老王的云盘";
});

app.controller('XiaowangCtrl', function ($scope) {

    $scope.Avideo = "小王的AV";
    $scope.cloudObj.Avideo = "小王的云盘";
})
```

### 我们来分析一下这段代码：
+ 控制器嵌套，可以看到“小王控制器”嵌套在“老王控制器”里。
+ “小王scope”继承自“老王scope”。
+ 父子俩的控制器里都定义了Avideo和cloudObj.Avideo，然后在视图中输出他们。

### 我们先来看一下运行效果：
![](http://7xtoaz.com1.z0.glb.clouddn.com/scope-in-angular-001.png)

![](http://7xtoaz.com1.z0.glb.clouddn.com/mengbi4.jpg)
纳尼？！！小王的云盘怎么跑到老王那里了！

终于可以引出问题了，当使用Angular嵌套controller的时候，由于每一个controller都有自己的scope（也就是作用域、控制范围），所以Controller的嵌套也意味着scope的嵌套。如果两个控制器都有相同的Model（比如上栗中，父子俩都有相同的兴趣爱好）会发生什么呢？子控制器是继承还是更新父控制器？

看到这里先卖个关子，让我们聊聊JavsScript的对象、原型和原型链。

### JavsScript中的对象
众所周知，在JavsScript中，万物皆对象！但对象也略有不同。在JavsScript中，对象分为<b>普通对象</b>和<b>函数对象</b>。来看段代码：

```
//function func1(){};
var func2 = function(){};
var func3 = new Function("str", "console.log(str)");

var obj1 = {};
var obj2 = new Object();
var obj3 = new f1();

console.log(typeof Object);//function
console.log(typeof Function);//function
console.log(typeof obj1);//object
console.log(typeof obj2);//object
console.log(typeof obj3);//object
console.log(typeof func1);//function
console.log(typeof func2);//function
console.log(typeof func3);//function
```

在上面的代码中，obj1、obj2、obj3为普通对象，func1、func2、func3为函数对象。

区分两者也很简单，
+ 通过new Function()创建的对象都是函数对象。
+ 除此之外的都是普通对象。

func1、func2归根结底都是通过new Function()的方式创建的。Function Object也是通过new Function()的方式创建的。

### JavsScript中的原型
在JavsScript中，每当定义一个对象（普通对象）时，JavsScript语言本身会自动为该对象创建一些属性，其中一个属性就是prototype（即原型）。

<b>注意：普通对象没有prototype属性，但有`__proto__`属性，我们待会儿再聊`__proto__`属性</b>

我们来看段代码：

```
function func1(){};
var func2 = function(){};
var func3 = new Function("str", "console.log(str)");

var obj1 = {};
var obj2 = new Object();
var obj3 = new f1();

console.log(obj1.prototype);//undefined
console.log(func1.prototype);//function func1(){}
console.log(typeof func1.prototype);//object
console.log(Function.prototype);//function(){}
console.log(typeof Function.prototype);//fuction
console.log(typeof Object.prototype);//object
console.log(typeof Function.prototype.prototype);//undefined
```

上面讲过了，obj1、obj2、obj3为普通对象，func1、func2、func3为函数对象。

我们从
```
console.log(obj1.prototype);//undefined
```

可以看出，普通对象并没有prototype属性。

我们从
```
console.log(func1.prototype);//function func1(){}
```
可以看出，func1.prototype就是func1的一个实例对象。也就是在func1创建的时候发生了以下事情：
+ `var temp = new func1()`
+ `f1.prototype = temp`
是不是发现了什么？

```
console.log(Function.prototype);//function(){}
```
在这行代码中，为什么Function.prototype是函数对象呢？正如上面你看到的一样，在Function创建的时候，创建了一个它的实例对象并且赋值给了它的prototype。

即：

```
 //var temp1 = new Function ();
 //Function.prototype = temp1;
```

简而言之，对象的原型就是该对象的一个实例对象。

那么原型有什么作用呢？答案是继承。

我们来看一段代码：

```
var laowang = function(videoName){
    this.videoName = videoName;
}
parent.prototype.getVideoName = function(){
    return this.videoName;
}
var xiaowang = new laowang("Tokyo-hot");
xiaowang.getVideoName();//Tokyo-hot
```

上面这段代码并不难，我们可以看到，在xiaowang中并没有getVideoName()方法，但是xiaowang从laowang哪里继承了getVideoName()方法。具体是怎么继承的，我们接着来聊。

### JavsScript中的原型链

#### 注意
+ 原型链依靠`__proto__`形成，而不是prototype。
+ `prototype`代表了对象的原型，普通对象没有prototype属性。
+ `__proto__`和`prototype`不是一回事，两者的作用不同。 
+ `__proto__`指向于它所对应的原型对象（Chrome、Firefox中名称为`__proto__`，并且可以被访问到）。
+ JavsScript中的每个对象都和其他对象相关联（NULL除外）。

JavsScript在创建对象的时候都有一个`__proto__`属性，这个属性不像prototype，普通对象是没有prototype属性的，但是每个对象都有`__proto__`属性。

我们接着上面的例子：

```
var laowang = function(videoName){
    this.videoName = videoName;
}
parent.prototype.getVideoName = function(){
    return this.videoName;
}
var xiaowang = new laowang("Tokyo-hot");
xiaowang.getVideoName();//Tokyo-hot

console.log(xiaowang.__proto__ === laowang.prototype);//true
console.log(laowang.prototype.__proto__ === Object.prototype);//true
console.log(Object.prototype.__proto__);//null，比较特殊
```

上面的例子可以以一张图形象的表示：

![](http://7xtoaz.com1.z0.glb.clouddn.com/scope-in-angular-002.png)
+ `xiaowang.__proto__`指向`laowang.prototype`
+ `laowang.prototype__proto__`指向`Object.prototype`
+ `Object.prototype.__proto__`指向`null`

我们把这个由`__proto__`串起来的直到Object.prototype.`__proto__`为`null`的链叫做<b>原型链</b>。

我们再来看一段代码：

```
console.log(Object.__proto__ === Function.prototype); // true
console.log(Function.__proto__ === Function.prototype); // true
console.log(Function.prototype.__proto__ === Object.prototype); //true
```

先来看第一行：

从
```
console.log(typeof Object);//true
```
我们知道Object是由Function创建的，也可以说Object继承自Function，也就是说`Object = new Function(){}`，由原型链的继承机制我们可以轻松理解上面这段代码的第一行
```
console.log(Object.__proto__ === Function.prototype); // true
```

再来看第二行
同样的Function也是函数对象，也是通过`new Function(){}`创建的，所以
```
console.log(Function.__proto__ === Function.prototype); // true
```
也是成立的。

博主：“啥玩意儿？自己创建自己？？”
![](http://7xtoaz.com1.z0.glb.clouddn.com/mengbi.jpg)

好像很不符合逻辑，其实现实世界也有类似，你是你妈生的，你妈是你姥姥生的...类人猿进化来的，那么类人猿从哪里来...一直追溯下去，就像原型链的追踪一样，最终就是null（无）。

最后看第三行：
```
console.log(Function.prototype.__proto__ === Object.prototype); //true
```
这又是啥玩意儿？`Function.prototype`不是函数对象吗？它的`__proto__`不应该指向`Function.prototype`吗？
我们可以理解为JavsScript为了保证原型链能够正常结束，函数也是对象，所以让Function.prototype.`__proto__`指向`Object.prototype`，并且让`Object.prototype.__proto__`指向`null`。

### Function、Object、Prototype、`__proto__`之间的关系
上代码：

```
function Animal(){
    
}

var anim = new Animal();
console.log('***********Animal anim proto*****************');
console.log(typeof Animal.prototype);//object
console.log(anim.__proto__ === Animal.prototype);//true
console.log(Animal.__proto__ === Function.prototype);//true
console.log(Animal.prototype.__proto__ === Object.prototype);//true

console.log('***********Function proto*****************');
console.log(typeof Function.prototype);//function
console.log(typeof Function.__proto__);//function
console.log(Function.prototype.prototype);//undefined
console.log(typeof Function.prototype.__proto__);//object

console.log('***********Object proto*****************');
console.log(typeof Object.prototype);//object
console.log(typeof Object.__proto__);//function
console.log(Object.prototype.prototype);//undefined
console.log(Object.prototype.__proto__);//null

console.log('***********Function Object  proto关系*****************');
console.log(Function.prototype === Object.__proto__);//true
console.log(Function.__proto__ === Object.__proto__);//true
console.log(Function.prototype.__proto__ === Object.prototype);//true

console.log('**************内置对象Array、Date****************'); 
var array = new Array();
var date = new Date();

console.log(array.__proto__ === Array.prototype);//true
console.log(Array.__proto__ === Function.prototype);//true

console.log(date.__proto__ === Date.prototype);//true
console.log(Date.__proto__ === Function.prototype);//true
```

上述代码的内存关系图：
![](http://www.blogjava.net/images/blogjava_net/heavensay/web-front/8164530.png)

内存关系图说明：
![](http://www.blogjava.net/images/blogjava_net/heavensay/web-front/35166462.png)

注意：
+ 理解JavsScript中的栈和堆可以参考博文[《理解JavsScript内存分配》](http://www.cnblogs.com/fool/archive/2010/10/07/1845226.html)
+ Function.prototype函数对象图内部表示prototype属性的红色虚框，只是为了说明这个属性不存在。
+ 所有对象，包括函数对象的原型链最终都指向了Object.prototype，而
```
Object.prototype.__proto__===null
```
，原型链至此结束。
+ Animal.prototype是一个普通对象。
+ Object是一个函数对象，也是Function构造的，Object.prototype是一个普通对象。
+ `Object.prototype.__type__`指向null。
+ Function.prototype是一个函数对象，前面说函数对象都有一个显示的prototype属性，但是Function.prototype却没有prototype属性，即
```
Function.prototype.prototype===undefined
```
所有Function.prototype函数对象是一个特例，没有prototype属性。
+ Object虽是Function构造的一个函数对象，但是Object.prototype没有指向Function.prototype，即
```
Object.prototype!==Function.prototype。
```

### Prototype跟Constructor的关系
在 JavaScript 中，每个函数对象都有名为“prototype”的属性(上面提到过Function.prototype函数对象是个例外，没有prototype属性)，用于引用原型对象。此原型对象又有名为“constructor”的属性，它反过来引用函数本身。这是一种循环引用（i.e. 
```
Animal.prototype.constructor===Animal
```
）。

上代码：

```
console.log('**************constructor****************'); 
console.log(anim.constructor === Animal);//true
console.log(Animal === Animal.prototype.constructor);//true
console.log(Animal.constructor === Function.prototype.constructor);//true
console.log(Function.prototype.constructor === Function);//true
console.log(Function.prototype.constructor === Function.constructor);//true

console.log(Object.prototype.constructor === Object);//true
console.log(Object.constructor === Function);//true
```

内存关系图：

![](http://www.blogjava.net/images/blogjava_net/heavensay/web-front/8199006.png)

注意：
+ 红色箭头表示函数对象的原型的constructor所指向的对象。
+ 注意Object.constructor===Function；本身Object就是Function函数构造出来的 
+ 如何查找一个对象的constructor，就是在该对象的原型链上寻找碰到的第一个constructor属性所指向的对象。



未完待补充。。。