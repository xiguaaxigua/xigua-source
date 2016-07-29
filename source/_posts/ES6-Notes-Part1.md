---
title: 初涉ES6
date: 2016-07-06 22:58:40
tags: [JavaScript]
---
记录一下ES6的学习过程。
<!--more-->

<audio src="http://7xtoaz.com1.z0.glb.clouddn.com/myHumps.aac" controls></audio>

本文是学习[阮一峰](http://www.ruanyifeng.com/home.html)的《[ECMAScript 6入门](https://www.gitbook.com/book/wohugb/ecmascript-6/details)》一书的笔记。

## 什么是ES6
+ ES6是JavaScript语言的第6代标准，已经在2015年6月发布。ES6的目标是使得JavaScript可以用来编写复杂的大型应用，成为企业级开发语言。
+ JavaScript的历史版本为1、2、3、5。ES4由于太激进被中止开发。
+ 标准的制定者计划每年发布一次新版本，使用年份作为标准的版本。因此ES6又是ES2015。
+ 浏览器对ES6的支持情况，[点击访问](http://kangax.github.io/compat-table/es6/)。

### 通过es-checker模块检测浏览器对ES6的支持情况
```
npm install -g es-checker
es-checker

//博主本机运行结果为
=========================================
Passes 29 feature Detections
Your runtime supports 69% of ECMAScript 6
=========================================
```

### Babel转码器
将ES6代码转换为ES5代码。这意味你，你可以以ES6的方式开发，又不用担心浏览器的支持情况。
+ [Babel官网](https://babeljs.io/)
+ [Babel学习笔记]()
+ [在线转换](https://babeljs.io/repl/)

### ECMAScript和JavaScript的关系
+ ECMA是一个国际化标准组织；
+ ECMAScript是JavaScript的标准；
+ JavaScript是ECMAScript的实现；
+ ECMAScript又称为JScript或者ActionScript。

## ES6的新特性
### 1.Arrow
简化匿名函数
```
document.getElementById('#demo').onclick = function(){
    console.log('Hello JavaScript!');
}
```

简化为：
```
document.getElementById('#demo').onclick = v => console.log('Hello ES6!');
```

### 2.Class
ES6中添加了对类的支持，引入了`Class`关键字。JavaScript本身就是面向对象的， ES6中提供的类实际上只是JS原型模式的包装。现在添加原生的`class`支持后，对象的创建，继承更加直观了，并且父类方法的调用、实例化、静态方法和构造函数等概念都更加形象化。

```
//创建一个类
class Father(){

    //构造函数
    constructor(name){
        this.name = name;
    }

    //实例方法
    sayName(){
        console.log('I`m ' + this.name);
    }
}

//继承
class Sun extends Father(){
    constructor(){

        //直接调用父类的构造器进行初始化
        super(name);
    }
    sayFuck(){
        console.log('Fuck' ＋ this.name);
    }
}

//实例化Father
var f1 = new Father('老王');
var s1 = new Sun('小王');

//调用类中的方法
f1.sayName();
s1.sayName();
s1.sayFuck();
```

### 3.增强的对象字面量

什么是对象字面量？ 
+ 字面量是一种表示值的方法。例如：“Hello World”在许多语言中都表示一个字符串字面量。
+ 在JavaScript中，例如：5、`false`和`null`分别表示一个整数、布尔值和空对象。
+ 对象字面量是一个名/值对列表，每个名和值之间用逗号间隔。

对象字面量被增强了，写法更加简洁和灵活，同时在定义对象的时候能够做的事情更多了，具体表现在：
+ 可以在对象字面里面定义原型；
+ 定义方法可以不用`function`关键字；
+ 可以直接调用父类中的方法。

```
//通过对象字面量创建对象
var Human = {
    
    eat(){
        console.log('I`m eating...');
    }
};

var Student = {
    __proto__: Human,
    school: 'TYUT',
    study(){
        console.log('I`m reading...');
    },
    getSchool(){
        console.log(this.school);
    }
};

Human.eat();
Student.eat();
Student.study();
Student.getSchool();
```

### 4.字符串模板
字符串模板相对简单易懂一些。

```
var name = '小王';

//注意是反引号，而不是单引号
//变量的格式：${name}
console.log(`我的名字是${name}`);
```

### 5.解构
#### _5.1_ 自动解析数组或者对象中的值。例如：
```
//bad
var a = 1;
var b = 2;
var c = 3;

//good 
var [a, b, c] = [1, 2, 3]
```

#### _5.2_ 如果解构失败，将会返回 `undefined` 。例如：
```
//解构不成功，a的值为undefined
var [a] = [];

//解构不成功，b的值为undefined
var [a, b] = [1];
```

#### _5.3_ 不完全解构：不匹配，但是可以解构成功。
```
var [a, b] = [1, 2, 3];
x //1
y //2

var [a, [b], c] = [1, [2, 22], 3];
a //1
b //2
c //3
```

#### _5.4_ 不能解构的情况：
```
var a = 1;
var b = false;
var c = NaN;
var d = undefined;
var e = null;
var c = {};
```

#### _5.5_ 解构赋值不仅适用于 `var` ，也适用于 `const` 和 `let` 。

#### _5.6_ 默认值
```
var [a, b = 2] = [1];
a //1
b //2

/*
ES6内部使用严格相等运算符（`===`）,判断一个位置是否有值。
即：如果值===undefined，那么这个值是不生效的。依旧为默认值。
undefined === undefined;因此a的值为默认值。
*/
var [a, b = 2] = [1, undefined];
a //1
b //2

//null !=== undefined;因此a的值为null
var [a, b = 2] = [1, null];
a //null
```

### _5.7_ 字符串的解构赋值
```
//字符串被转换为了一个类似数组的对象
var [a, b, c, d, e] = 'Hello';
a //H
b //e
c //l
d //l
e //o

//类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。
var {length: len} = 'Hello';
len //5
```

### _5.8_ 布尔值、数字、函数等都可以进行解构赋值
用到的时候再研究吧。

### _5.9_ 解构赋值的用途
+ 交换变量的值
    ```
        [x, y] = [y, x];
    ```
    
+ 从函数返回多个值
    ```
    var [firstName, northWind, autumnWind] = getInfo();
    function getInfo(){
        return ['老王', '北风', '秋风'];
    }
    console.log( northWind + "吹，" + autumnWind + "凉，谁家娇妻守空房；" );
    console.log("你有难，我帮忙，我住隔壁我姓" + firstName + "！");
    ```
    
+ 定义函数的参数
    ```
    //貌似并没有什么卵用
    function getInfo([name, age, money]){
        
        console.log(name, age, money);
    }
    getInfo(['老王', '22', '$100']);//老王 22 $100
    ```

+ 提取JSON数组
    ```
    var dataJson = {
        name: '老王',
        age: '22',
        money: '$100'
    }
    
    var {name, age, money} = dataJson;
    
    console.log(name, age, money);//老王 22 $100
    ```
    
+ 函数参数的默认值
+ 遍历Map结构
+ 输入模块的指定方法
    
### 6.let
#### _6.1_ let相当于var，但是let只有在它所在的代码块生效。
```
{
	var a = 1;
	let b = 2;
}
console.log(a);//1
console.log(b);//Uncaught ReferenceError: b is not defined

//let很适用于for循环  
for( let i=0; i<10; i++){}
console.log(i);//Uncaught ReferenceError: i is not defined
```

#### _6.2_ let声明的变量不存在变量提升。
```
console.log(aa);//undefined
console.log(bb);//Uncaught ReferenceError: bb is not defined

var aa = 1;
let bb = 1;
```
+ 变量aa用 `var` 命令声明，在脚本开始运行时已经存在了，但是没有值，所以会输出 `undefined` 。
+ 变量bb用 `let` 命令声明，不会发生变量提升，这表示在声明它之前，变量var是不存在的。因此，变量一定要先声明再使用。

#### _6.3_ 暂时性死区
暂时性死区（Temporal Dead Zone，简称TDZ）：在代码块内，使用let命令声明变量之前（let命令之前的区域即“TDZ”），该变量都是不可用的。这在语法上称为“暂时性死区”。

```
var aaa = 111;
if(1){

	aaa = 'aaa';
	let	aaa;//Uncaught ReferenceError: aaa is not defined
}
```
如果区块中存在let和const命令，这个区块里使用这些命令声明的变量，从一开始就形成了封闭作用域。
**凡是在声明之前就是用这些变量将会报错**。

TIPS：
+ ES6规定了暂时性死区和不存在变量提升，主要是为了避免变量在未声明之前使用引起的错误，从而导致意外的错误。
+ 暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取。只有在声明之后才可以使用。

#### _6.4_ 不允许重复声明
let不允许在相同作用域内，重复声明同一个变量。

### 7.块级作用域
#### _7.1_ ES5只有全局作用域和函数作用域，这造成了很多不合理的场景。
场景一，内层变量覆盖外层变量：
```
var d = new Date();

function func(){
    console.log(d);
    if(0){/
        var d = 'Hello ES5!';
    }
}

/* 变量提升，内层的变量覆盖了全局变量 *
func();//undefined
```

场景二，用来计数的循环变量泄露为全局变量：
```
var welcome = 'Hello ES5';
for (var i=0; i< welcome.length; i++){
    console.log(welcome[i]);
}

/* i只用来循环，但是循环结束以后，并没有消失，而是泄露为全局变量*/
console.log(i);//9
```

#### _7.2_ ES6的新增的块级作用域。
```
function func(){

    let a = 'Hello';
    if(a){
        //内层作用域可以定义与外层作用域同名的变量，内层并不会覆盖外层，而是独立于一个作用域。
        let a = 'Hello ES6';
    }
    
    console.log(a);
}
func();//Hello
```

#### _7.3_ 块级作用域与函数声明。
在ES5中，只允许在顶层作用域和函数作用域内声明函数。下边代码在严格模式下报错。
```
'use strict';
if(1){
    function func(){
        //代码体
    }
}
```

在ES6中，允许在块级作用域内声明函数。上面代码中ES6下不报错。

#### TIPS：
1.应避免在块级作用域内声明函数，如果确实需要，应该写成表达式的形式，而不是函数声明语句。
```
//函数声明语句
{
    let a = 'Hello';
    function func(){
        return a;
    }
}

//函数表达式
{
    let a = 'Hello';
    let f = function(){
        return a;
    }
}
```

2.ES6的块级作用域允许声明函数的规则，只在大括号的情况下成立。
```
//不报错
'use strict';
if(true){
    function func(){}
}

//报错
'use strict';
if(true)
    function func(){}
```

3.ES6允许块级作用域的任意嵌套。

4.不同作用域之间的变量是相互隔绝的，不可以互相引用，可以同名。

5.块级作用域的出现，使得立即执行函数不再必要了。

_什么是立即执行函数？_
立即执行函数（Immediately Invoked Function Expression，即“IIFE”），[JavaScript的IIFE](http://rensanning.iteye.com/blog/2080429)。
```
//标准写法
(function(){
    //代码体
    
})();
```

### 8.const
+ 用来定义常量，无法别更改。
+ 常量一旦被声明，就必须被初始化。
+ const的作用域和let相同，只在块级作用域内有效。
+ const和let一样，同样存在TDZ。
+ const和let一样，不可以被重复声明。
+ 对于复合类型的变量，变量名不指向数据，而是指向数据的地址，const只保证地址不变，不保证数据不变。因此在声明一个复合类型的数据为常量的时候要小心。

_冻结对象？_
冻结对象可以使用 `Object.freeze` 方法。
冻结对象及对象内的方法：
```
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, value) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

### 9.Symbol
ES5中的一个会引起冲突的情况：
开发A写了一个对象 `userInfo` ， `userInfo` 中有一个获取用户手机号的方法 `getUserInfo()`，开发B想实现一个获取用户年龄的方法，也命名为 `getUserInfo()`，很明显此时就会发生错误。

ES6引入了Symbol方法就是为了防止属性名的冲突。
+ Symbol是一种新的原始数据类型，表示独一无二的值。它是JavaScript语法的第七种数据类型，其余六种是：`String` 、`Number` 、 `Boolean` 、 `Object` 、 `Null` 、 `Undefined` 。
+ Symbol值通过`Symbol`函数生成，这就是说，对象的属性名现在可以有两种类型，一种是原来的字符串，另一种就是新增的Symbol类型。凡是属性名属于Symbol类型，就是独一无二的，不会与其它属性名冲突。
+ Symbol是一种类似于字符串的类型。
```
var s1 = Symbol();
console.log(typeof s1);//symbol
```


### 10.for of 遍历
我们都知道`for in`用于遍历数组、类数组或者对象。ES6引入了类似于`for in`的`for of`，不同的是， 每次循环提供的不是键，而是值。

```
var arr = ["a", "b", "c"];
for (v of arr){
    console.log(v);//a,b,c
}
```

### 11.Module
在ES6中，JavaScript支持`module`！这种将JS代码分割成不同功能的小块进行模块化的概念是在一些第三方规范中流行起来的，例如：CommonJS和AMD模式。

将不同的代码分别写在不同的文件里，各模块只需导出公共接口部分，然后通过模块的导入的方式可以在其他地方使用。

```
//getName.js
module "getName"{
    export class GetName{
        
        constructor(name){
            this.name = name;
            return this.name;
        }
    }
}

//myApp.js
//声明引用的模块
module getName from "/getName.js";

import GetName from "getName";

var g1 = new GetName("老王");

console.log(g1);
```

## End
ES6引入了很多新特性，让前后端的差异越来越小，此部分欠缺甚多，需更加努力。