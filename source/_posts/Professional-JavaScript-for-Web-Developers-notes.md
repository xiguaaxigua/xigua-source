---
title: JavaScript高级程序设计（第3版）-读书笔记
date: 2016-07-22 15:56:20
tags: [JavaScript, 读书笔记]
---

记录一下那些年错过的“爱情”（知识点）。
<!-- more -->

看书的过程中随手记下的知识点，内容杂乱无序。
## 正文

1.标识符：指变量、函数、属性的名字，或者函数的参数。标识符中的字母也可以包含扩展的ASCII或者Unicode字母，但是不推荐使用。

2.ASCII：美国信息交换标准代码，是基于拉丁字母的一套电脑编码系统，主要用于显示现代英语和其他西欧语言。

3.Unicode：计算机科学领域里的一项业界标准，包括字符集、编码方案等。Unicode是为了解决传统的字符编码方案的局限而产生的，它为每种语言中的每个字符设定了统一并且唯一的二进制编码，以满足跨语言、跨平台进行文本转换、处理的要求。

4.松散类型：可以用来保存任何类型的数据。

5.不建议修改变量所保存的值的类型。
```
var message = 'hello';
message = 10086; // 有效，但不推荐
```

6.不推荐通过省略var操作符的方式定义全局变量。原因：
+ 难以维护；
+ 严格模式下报错。

7.五种简单数据类型（也叫基本数据类型）：Undefined、Null、Boolean、Number、String。一种复杂数据类型：Object。

8.不推荐显式地把变量设置为 `undefined` 。`undefined` 主要用于比较。

9.null表示一个空对象指针。undefined表示未经初始化的变量。

10.保存浮点数所需的内存空间是整数的两倍，JavaScript会不失时机的将浮点数转化为整数。
```
var num1 = 1.0;
console.log(num1); // 1
var num2 = 10.0;
console.log(num2); // 10
```

11.科学计数法（e表示法）：
```
console.log(3.1415e6); // 3141500 
console.log(3e-6); // 0.000003
```

12.不要测定某个特定的浮点数值。
```
console.log(0.1 + 0.2 == 0.3); // false
```

浮点数值的最高精度是17为小数，但在进行算数计算时其精确度远远不如整数。例如，0.1加0.2的结果不是0.3而是0.30000000000000004。这个小小的摄入误差会导致无法测定特定的浮点数值。

13.最小数值：Number.MIN_VALUE。最大数值：Number.MAX_VALUE。

如果某次计算的结果得到了一个超出JavaScript数值范围的值，那么这个数值将被自动转换成特殊的Infinity值。正无穷：Infinity，负无穷：-Infinity。

14.toString()方法可以接受参数：
```
var num = 10;
console.log(num.toString(2)); // "1010"，二进制
console.log(num.toString(8)); // "12"， 八进制
console.log(num.toString(10)); // "10"， 十进制，默认。
console.log(num.toString(16)); // "a"， 十六进制
```

15.Number、Boolean、Object、String都有toString()方法。

16.在不知道要转换的值是否为null或者undefined的情况下，可以使用转型函数String()。
```
var v1 = 10;
var v2 = true;
var v3 = {name: '老王'};
var v4 = 'Hello';
var v5 = null;
var v6;
console.log(String(v1)); // "10"
console.log(String(v2)); // "true"
console.log(String(v3)); // ""[object Object]""
console.log(String(v4)); // "Hello"
console.log(String(v5)); // "null"
console.log(String(v6)); // "undefined"
```

17.Object的每个实例都具有以下属性和方法：
+ Constructor：保存着用于创建当前对象的函数。
+ hasOwnProperty：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。
+ isPrototypeOf(obj)：用于检查传入的对象是否是另一个对象的原型。
+ propertyIsEnumerable：用于检查给定的属性是否能够使用for-in语句来枚举。
+ toLocaleString()：返回对象的字符串表示，该字符串与执行环境的地区对应。
+ toString()：返回对象的字符串表示。
+ valueOf()：返回对象的字符串、数值或者布尔值表示。通常与toString()方法的返回值相同。

18.在函数体内可以通过arguments对象来访问传入该函数的参数：（这说明在JavaScript中参数命名只是为了方便，而不是必须的）
```
var a = 1;
var b = 2;
var c = 3;

function getArguments(){
	console.log('第一个参数是：' + arguments[0]);
	console.log('第二个参数是：' + arguments[1]);
	console.log('第三个参数是：' + arguments[2]);
}
getArguments(a, b, c);
```

19.JavaScript中的函数不能像传统意义那样实现重载。而在其他语言中，可以为一个函数编写两个定义，只要这两个函数的签名（接受的参数的类型和数量）不同即可。JavaScript中的函数没有签名，其参数由包含零个或多个值的数组来表示的，而没有签名的函数，真正的重载是不可能做到的。只能实现类似的重载：
```
function hello(){
	if(arguments.length == 1){
		// 代码体A
	}else if(arguments.length == 2){
		// 代码体B
	}
}
```

如果在JavaScript中定义了两个名字相同的函数，后者会覆盖前者。
```
function hello(){
	console.log('Hello FuncA');
}

function hello(){
	console.log('Hello FuncB');
}

hello(); // Hello FuncB
```

20.ECMAScript中包含了所有基本的语法、操作符、数据类型以及完成基本的计算任务所必须的对象，但没有对取得输入和产生输出的机制做出规定。理解ECMAScript及其纷繁复杂的细节，是理解其在Web浏览器中的实现——JavaScript的关键。

ECMAScript中基本的要素：
+ ECMAScript中的基本数据类型包括：Undefined、Null、Boolean、Number、String。
+ 与其他语言不同，ECMAScript没有为整数和浮点数值分别定义不同的数据类型，Number可用于表示所有数值。
+ ECMAScript中也有一种复杂的数据类型，即Object类型，该类型是这门语言中所有对象的基础类型。
+ 严格模式为这门语言中容易出错的地方施加了限制。
+ ECMAScript提供了很多与C及其他类C语言中相同的基本操作符，包括算数操作符、布尔操作符、关系操作符、相等操作符及赋值操作符等。
+ ECMAScript从其他语言中借鉴了很多流控制语句，例如if语句、for语句、switch语句等。

ECMAScript与其他语言也有很多不同之处：
+ 无需指定函数的返回值，因为任何ECMAScript函数都可以在任何时候返回任何值。
+ 实际上，未指定返回值的函数返回的是一个特殊的undefined值。
+ ECMAScript中也没有函数签名的概念，因为其函数参数是以一个包含零个或多个值的数组的形式传递的。
+ 可以向ECMAScript函数传递任意数量的参数，并且可以通过arguments对象来访问这些参数。
+ 由于不存在函数签名的特性，ECMAScript函数不能重载。

21.ECMAScript变量可能包含两种不同数据类型的值：基本类型值和引用类型值。基本类型值指的是简单的数据段，引用类型值只的是那些可能由多个值构成的对象。

引用类型的值是保存在内存中的对象。与其他语言不同，JavaScript不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间。在操作对象时，实际上实在操作对象的引用而不是实际的对象。为此，引用类型的值是按引用访问的。