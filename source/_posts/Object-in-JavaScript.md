---
title: 温习JavaScript中的对象
date: 2016-06-04 16:09:23
tags: [JavaScript]
---
总结一下。
<!--more-->

### 温故而知新
博主始终相信学习是一个长期积累的结果，这篇文章结束以后，感觉对JavaScript的对象这块清晰了许多，也解开了之前的一些疑惑。“温故而知新，可以为师矣。”古人诚不欺我。

### 基本概念
JavaScript中的数据类型：
+ 原始类型
+ 对象类型

原始类型包括：
+ Number
+ String
+ Boolean
+ 特殊的原始值
	+ Null
	+ Undefined

对象类型包括：
+ Object
+ 特殊对象
	+ Array
	+ Function

JavaScript中数据类型的另一种划分：
+ 可变类型（mutable）
	+ Object
	+ Array

+ 不可变类型（immutable）
	+ Number
	+ Boolean
	+ Null
	+ Undefined
	+ String

JavaScript中的变量：
+ 全局变量：不在任何函数内声明的变量
+ 局部变量：在函数体内声明的变量，具有函数作用域，只在函数内可见

JavaScript中的对象：
+ 对象是从字符串到值的映射
+ 并非“万物皆对象”
+ 除了Number、String、Boolean、Null、Undefined外，都是对象
+ Number、String、Boolean的行为和不可变对象很类似

对象的属性特性：
+ 可写（writable attibute）
+ 可枚举（enumerable attribute）
+ 可配置（configurable attribute）

对象的对象特性：
+ 原型（prototype）
+ 类（class）
+ 扩展标记（extensible flag）：标识该对象是否可扩展，即是否可以向该对象添加新属性

JavaScript中的三类对象和两类属性：
+ 三类对象
	+ 内置对象：JavaScript语言本身内置的对象。
	+ 宿主对象：宿主环境定义的对象。
	+ 自定义对象：用户自定义的对象。
+ 两类属性
	+ 自有属性：该对象本身的属性，即在对象中定义的属性。
	+ 继承属性：继承来的属性，即在该对象的原型中定义的属性。

此处应该有张脑图：
![](http://7xtoaz.com1.z0.glb.clouddn.com/Object-in-JavaScript-02)

小贴士：
+ 用来初始化一个新对象的函数，成为构造函数。
+ 数组：表示带编号的值的集合。数组的使用频率很高，JavaScript单独将数组划分出来，是为了让数组拥有一些和普通对象不同的特性。
+ 函数：具有与它相关联的可执行代码的对象。和数组一样，函数也有和普通对象不同的特性。
+ 类：可以看做对象类型的子类型。JavaScript中的类有：Array类、Function类、Date类、RegExp类、Error类。

问题：
+ 为什么Number、String是不可变类型？答：修改一个数字本身就是行不通的。字符串同理。

## 对象的常见用法

### 创建对象
创建对象的三种方式：
+ 对象直接量
+ 通过new关键字
+ Object.create()方法

演示：
```
//对象直接量
var obj1 = {name: 'Xigua', sex: 'male'};
console.log(typeof obj1);//object

//new关键字
var obj2 = new Object();
obj2.name = 'Xigua';
obj2.sex = 'male';
console.log(typeof obj2);//object

//Object.create()方法
var obj3 = Object.create({name: 'Xigua', sex: 'male'});
console.log(typeof obj3);//object
```

小贴士：
+ new关键字用于创建并初始化一个新对象。new关键字后的函数即构造函数。构造函数的作用是初始化新创建的对象，即：将构造函数的原型赋给新对象的原型。<b>JavaScript中的原始类型都包含内置构造函数</b>。
+ <b>通过对象直接量创建的对象都具有同一个原型对象</b>。即：通过这种方式创建的对象的prototype都指向`Object.prototype`。
```
//对象直接量
var obj1 = {name: 'Xigua', sex: 'male'};
console.log(typeof obj1);//object

console.log(obj1.__proto__ === Object.prototype);//true
/*
为什么这里不是console.log(obj1.prototype === Object.prototype);？
此处留作疑问，下面单独开个标题讨论prototype和__proto__的区别。
*/
```

### 属性的查询和设置

```
var obj1 = {name: 'Xigua', sex: 'male'};

//查询
console.log(obj1.name);//Xigua
//或者
console.log(obj1['name']);//Xigua

//设置
obj1.name = 'Hello';
//或者
obj1['name'] = 'Hello';
console.log(obj1.name);//Hello
```
注意：
+ 点运算符后的标识符不能是保留字。
+ 使用方括号时，方括号内的表达式必须返回字符串或者返回一个可以转换为字符串的值。

### 继承
JavaScript对象具有“自有属性”，也有一些属性是从原型对象继承而来的。

假设要查询对象o的属性x，如果o中不存在x，那么将会继续在o的原型对象中查询属性x。如果原型对象中也没有x，但这个原型对象也有原型，那么继续在这个原型对象的原型上执行查询，直到找到x或者找到一个原型是null的对象为止。可以看到，对象的原型属性构成了一个“链”，也就是我们常说的“原型链”。通过原型链可以实现属性的继承。

```
var o = {};//o从Object.prototype继承对象的方法
o.x = 1;//给o定义一个属性x
var p = inherit(o);//p继承o和Object.prototype
p.y = 2;//给p定义一个属性y
var q = inherit(p);//q继承p、o和Object.prototype
q.z = 3;//给q定义一个属性
var s = q.toString();//toString继承自Object.prototype
console.log(q.x + q.y);//3，x和y分别继承自o和p
```

现在假设给对象o的属性x赋值
+ 如果o中已经有了属性x（这个属性不是继承来的），那么这个赋值操作只改变这个已有属性x的值。
+ 如果o中不存在属性x，那么赋值操作给o添加一个新属性x。
+ 如果之前o继承自属性x，那么这个继承的属性就被新创建的同名属性覆盖了。

属性赋值操作首先检查原型链，以此判定是否允许赋值操作。

属性赋值要么失败、要么创建一个新属性、要么在原始对象中设置属性，但是有一个例外，如果o继承自属性x，而这个属性是一个具有setter方法的accessor属性，那么这是将调用setter方法而不是给o创建一个属性x。需要注意的是，setter方法是由对象o调用的，而不是定义这属性的原型对象调用的。因此如果setter方法定义任意属性，这个操作只是针对o本身，并不会修改原型链。

### 删除属性

delete运算符可以删除对象的属性。它的操作数应该是一个属性访问表达式。delete只是断开属性和宿主对象的联系，而不会去操作属性中的属性。

delete运算符只能删除自有属性，不能删除继承属性（要删除继承属性必须从定义这个属性的原型对象上删除它，而且这会影响到所有继承自这个原型的对象）。

当delete表达式删除成功或没有任何副作用（比如删除不存在的属性）时，它返回true。如果delete后不是一个属性访问表达式，delete同样返回true。

```
var o = {x= 1};
delete o.x;//删除x,返回true
delete o.x;//什么都没做，x已经不存在了，返回true
delete o.toString;//什么都没做，toString是继承来的，返回true
delete 1;//无意义，返回true
```

delete不能删除那些可配置性为false的属性。某些内置对象的属性是不可配置的，比如通过变量声明和函数声明创建的全局对象的属性。在严格模式中，删除一个不可配置属性会报一个类型错误。在非严格模式中，在这些情况下的delete操作会返回false。

```
delete Object.prototype;//不能删除，属性是不可配置的
var x = 1;//声明一个全局变量
delete this.x;//不能删除这个属性
function f(){};//声明一个全局函数
delete this.f;//不能删除全局函数
```

当在非严格模式中删除全局对象的可配置属性时，可以省略对全局对象的引用，直接在delete操作符后跟随要删除的属性名即可。

```
this.x = 1;//创建一个可配置的全局属性（没有var）
delete x;//将它删除
```

然而在严格模式中，delete后跟随一个非法的操作数，则会报出一个语法错误，因此必须显式指定对象及其属性。

```
delete x;//在严格模式下报语法错误
delete this.x;//在严格模式下正常工作
```

### 检测属性
JavaScript对象可以看做属性的集合，我们经常会检测集合中成员的所属关系——判断某个属性是否存在于某个对象中。可以通过in运算符、hasOwnPreperty()和propertyIsEnumerable()方法来完成这个工作，甚至仅通过属性查询也可以做到这一点。

```
//in运算符：检测左侧的属性名是否是对象的自有属性或者继承属性
var o = {x: 1};
console.log("x" in o);//true
console.log("y" in o);//false
console.log("toString" in o);//true：o继承toString属性

//hasOwnProperty：检测给定的名字是否是对象的自有属性
var o = {x: 1};
console.log(o.hasOwnProperty("x"));//true
console.log(o.hasOwnProperty("y"));//false
console.log(o.hasOwnProperty("toString"));//false：toString是继承属性

//propertyIsEnumerable：检测给定的名字是否是对象的自有属性，并且这个属性可枚举
var o = inherit({ y: 2});
o.x = 1;
console.log(o.propertyIsEnumerable("x"));//true
console.log(o.propertyIsEnumerable("y"));//false：y是继承来的
console.log(Object.prototype.propertyIsEnumerable("toString"));//false：不可枚举

//更简单的方法“!==”：用于判断一个属性是否是undefined
var o = {x: 1, z: undefined};
console.log(o.x !== undefined);//true
console.log(o.y !== undefined);//false
console.log(o.toString !== undefined);//true：o继承了toString属性
//注意：检测z是否存在只能用in，因为in可以区分不存在的属性和存在但是值为undefined的属性
console.log("z" in o);//true
console.log(o.z !== undefined);//false：返回false，但是z属性存在

//注意：上述代码中使用的是“!==”而不是“!=”，因为“!==”可以区分null和undefined
```

### 属性getter和setter

我们知道，对象的属性是由属性名、属性值和一组特性组成。在ES5中，属性值可以用一个或两个方法替代，这两个方法就是getter和setter。由getter和setter定义的属性成为“存取器属性”。它不同于“数据属性”，数据属性只有一个简单的值。

注意：
+ 存取器属性具有不可写性。
+ 如果属性同时具有getter和setter属性，那么它是一个读/写属性。
+ 如果属性只有getter属性，那么它是一个只读属性。
+ 如果属性只有setter属性，那么它是一个只写属性。读取只写属性只会返回undefined。

```
//定义存取器属性最简单的方法是使用对象直接量语法的一种扩展写法
var o = {
	//普通的数据属性
	data_prop: value,

	//存取器属性都是成对定义的函数
	//get accessor_prop(){/* 函数体 */}
	//set accessor_prop(){/* 函数体 */}
};
```

存取器属性定义为一个或两个属性同名的函数，这个函数定义没有使用function关键字，而是使用get或set。注意，这里没有使用冒号将属性名和函数体分隔开，但在函数体的结束和下一个方法或数据属性之间有逗号分隔。

<i>博主对这块不是太熟悉，有机会将会更深一步研究。</i>

### 对象的三个属性

前面聊过每个对象都有与之相关的三个属性：原型、类、扩展标识。

原型：

对象的原型主要用来实现继承。

要想检测一个对象是否是另一个对象的原型，应该使用isPrototypeOf()方法。

```
var p = {x: 1};//定义一个原型对象
var o = Object.create(p);//使用这个原型创建一个对象
p.isPrototypeOf(o);//true
Object.prototype.isPrototypeOf(o);//true：p继承自Object.prototype
```

Mozilla、Chrome实现的JavaScript对外暴露了一个专门命名为`__proto__`的属性，用以直接查询/设置对象的原型。注意；有的浏览器可能不支持这个属性（待查）。

类：

对象的类属性是一个字符串，用以标识对象的类型信息。很鸡肋的一个属性，浏览器并未提供相应的方法去查询它

可扩展性：

对象的可扩展性用以表示是否可以给对象添加新属性。
+ 所有内置对象和自定义对象都是显式可扩展的，宿主对象的可扩展性由解析JavaScript的引擎决定。
+ 判断对象是否可扩展：Object.esExtensible()。
+ 将对象转换为不可扩展：Object.preventExtensions()。
+ 将对象设置为不可扩展并且将对象的所有自有属性都设置为不可配置的：Object.seal()。
+ 更严格的锁定对象，即冻结对象：Object.freeze()。
+ 检测对象是否被冻结：Object.isFrozen()。

### 序列化对象

对象序列化是指将对象的状态转换为字符串，也可将字符串还原为对象。即，对象和JSON数组的转换。此方法工作中经常用到。
```
var o = {x: 1, y: 2, z: 3};
s = JSON.stringify(o);
p = JSON.parse(s);

console.log(s);//得到一个JSON字符串：{"x":1,"y":2,"z":3}
console.log(p);//得到一个对象：Object {x: 1, y: 2, z: 3}
```

### 对象的方法

前面聊过，所有的JavaScript对象都从Object.prototype继承属性（除了那些不通过原型显式创建的对象）。这些继承属性主要是方法。前面聊过hasOwnProperty()、propertyIsEnumerable()、isPrototypeOf()这三个方法，以及Object构造函数里定义的静态函数Object.create()和Object.getPrototypeOf()等。除此之外还有一下几种常见的方法。
+ toString()
+ toLocalString()
+ toJSON()
+ valueOf()

### prototype和`__proto__`的区别

JavaScript中的内置构造器：
+ Number
+ Boolean
+ String
+ Object
+ Function
+ Array
+ RegExp
+ Error
+ Date
+ Global
+ Arguments
+ Math
+ JSON（ES5新增）


#### 1.所有构造器/函数的`__proto__`都指向`Function.prototype`

“所有构造器/函数”是指JavaScript中的构造器，和所有的函数（包括自定义函数）。

```
console.log(Number.__proto__ === Function.prototype);//true
console.log(Boolean.__proto__ === Function.prototype);//true
console.log(String.__proto__ === Function.prototype );//true
console.log(Object.__proto__ === Function.prototype);//true
console.log(Function.__proto__ === Function.prototype);//true
console.log(Array.__proto__ === Function.prototype);//true
console.log(RegExp.__proto__ === Function.prototype);//true
console.log(Error.__proto__ === Function.prototype);//true
console.log(Date.__proto__ === Function.prototype);//true

/*
Global不能直接访问
Arguments仅在函数调用时由JavaScript引擎创建
Math、JSON是以对象的形式存在的，无需new。他们的__proto__指向Object.prototype

console.log(Math.__proto__ === Object.prototype);//true
console.log(JSON.__proto__ === Object.prototype);//true
*/

//自定义函数
function func1(){};

var func2 = function(){};

var func3 = new Function("str", "console.log(str)");

console.log(func1.__proto__ === Function.prototype);//true
console.log(func2.__proto__ === Function.prototype);//true
console.log(func3.__proto__ === Function.prototype);//true
```

上例说明了什么问题呢？所有的构造器都来自于`Function.prototype`，包括根构造器Object和Function本身。所有构造器都继承了`Function.prototype`的属性和方法。

`Function.prototype`也是唯一一个typeof XXX.prototype为 “function”的prototype。其它的构造器的prototype都是一个对象。如下：
```
console.log(typeof Function.prototype); //function
console.log(typeof Object.prototype);//object
console.log(typeof Number.prototype);//object
console.log(typeof Boolean.prototype);//object
console.log(typeof String.prototype);//object
console.log(typeof Array.prototype);//object
console.log(typeof RegExp.prototype);//object
console.log(typeof Error.prototype);//object
console.log(typeof Date.prototype);//object
console.log(typeof Object.prototype);//object
```

我们alert(Function.prototype)试一下可以发现，`Function.prototype`是一个空函数。

![](http://7xtoaz.com1.z0.glb.clouddn.com/Object-in-JavaScript-01)

```
console.log(Function.prototype.__proto__ === Object.prototype);//true
```
这说明所有的构造器都是普通的JavaScript对象，可以给构造器添加、删除属性等。同时它也继承了`Object.prototype`的所有方法，如：toString、valueOf、hasOwnProperty等。

最后`Object.prototype`的原型是谁？

```
console.log(Object.prototype.__proto__ === null);//true
```

“无”生万物，到顶了，为null。

#### 2.所有对象的`__proto__`都指向构造器的prototype

上面测试了所有内置构造器及自定义构造器的`__proto__`，下面再看看所有这些构造器的实例对象的`__proto__`指向谁？


```
var num = 1;
var bool = false;
var str = "Hello World";
var obj = {name: 'Xigua'};
var arr = [1,2,3];
var reg = /js/g
var date = new Date;
var err = new Error("Something is Error");

console.log(num.__proto__ === Number.prototype);//true
console.log(bool.__proto__ === Boolean.prototype);//true
console.log(str.__proto__ === String.prototype);//true
console.log(obj.__proto__ === Object.prototype);//true
console.log(arr.__proto__ === Array.prototype);//true
console.log(reg.__proto__ === RegExp.prototype);//true
console.log(date.__proto__ === Date.prototype);//true
console.log(err.__proto__ === Error.prototype);//true

//自定义的构造器
//function Animal(name){
	this.name = name;
};

var a1 = new Animal('IT-Dog');

console.log(a1.__proto__ === Animal.protype);//true

//a1是Animal的一个实例对象，a1的__proto__总是指向其构造器的prototype
```

对上例中的自定义构造器修改一下：

```
function Animal(name){
	this.name = name;
};

//修改Animal的原型
Animal.prototype.getName = function(){};

var a1 = new Animal('IT-Dog');

console.log(a1.__proto__ === Animal.prototype);//true
console.log(a1.__proto__ === a1.constructor.prototype);//true

//可以看到，a1.__proto__、Animal.prototype、a1.constructor.prototype都指向同一个对象。
```

再来修改一下：

```
//function Animal(name){
	this.name = name;
};

//重写Animal的原型
Animal.prototype = {
	getName: function(){}	
};

var a1 = new Animal('IT-Dog');

console.log(a1.__proto__ === Animal.prototype);//true
console.log(a1.__proto__ === a1.constructor.prototype);//false


这里重写了Animal的原型，发现a1.__proto__和a1.constructor.prototype不再相等。
这也很好理解，给Animal.prototype赋值的是一个对象直接量{getName: function(){}}，
使用对象直接量定义的对象构造器指向的是根构造器Object，
而Object.prototype的一个空对象{}，{}自然与{getName: function(){}}不相等。
```

注意：
+ `__proto__`在IE6/7/8/9中都不支持。
+ 普通对象没有`prototype`属性，但有`__proto__`。
+ 原型链的形成依靠`__proto__`，而不是`prototype`。
+ `__proto__`和`prototype`不是一回事，两者的作用不同。
+ `__proto__`指向于它所对应的原型对象（Chrome、Firefox中名称为`__proto__`，并且可以被访问到）。
+ JavsScript中的每个对象都和其他对象相关联（NULL除外）。


