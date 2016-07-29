---
title: JavaScript的数组中的一个坑
date: 2016-06-23 13:21:12
tags: [JavaScript]
---
“[] == false”和“if([])”的返回值。
<!--more-->

这个坑来的十分突然，以至于博主忙活了一早上都没有找出问题到底出在哪里了。先抛出个问题：在JavaScript中空数组到底是true还是false？

“实践出真知”，上代码：
```
//先定义个空数组
var emptyArr = [];
if(emptyArr == true){
    console.log("空数组等于true");
}else{
    console.log("空数组等于false");
}
```
可以看到运行结果为：空数组等于false。那么再来一段代码：
```
var emptyArr = [];
if(emptyArr){
    console.log("空数组等于真");
}else{
    console.log("空数组等于假");
}
```
纳尼？空数组等于真？！菜鸡博主已经Mengbility...

分析一下吧。

```
var emptyArr = [];
console.log(typeof emptyArr);//object

//既然是对象，那么用于判断条件时就会转换为true
if(emptyArr)console.log('真');//真

//但是如果与布尔值比较
console.log(emptyArr == true);//false
console.log(emptyArr == false);//true

//我们再把emptyArr转换为布尔类型试一下
console.log(Boolean(emptyArr));//true

//结果是true，那么emptyArr在于布尔值比较的时候发生了什么呢？
//原来任意值在与布尔值比较的时候都会将两边的值转化为Number类型。
Number(emptyArr);//0
Number(false);//0

//于是乎，emptyArr == false，即当空数组作为判断条件时，相当于true。
//当空数组与布尔值比较的时候相当于false。
//就有了更加懵逼的情况：
[] == ![];//true

//那么如何避免数组与布尔值比较的坑呢？可以先把数组转换为布尔值。

//再抛出个小问题：
console.log(new Array(1) == false);//true

console.log([undefined] == false);//true
```

最后，<b>空数组等于false，但是为真，任意值在于布尔值比较的时候都会将两边的值转换为Number类型</b>，记住这句话吧。
