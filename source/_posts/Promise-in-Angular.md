---
title: Angular中的Promise接口
date: 2016-05-18 10:44:36
tags: [Angular]
---
Angular的学习笔记。
<!--more-->

## 概要
+ promise是用来处理对象的一个接口。
+ 使链式调用变得清晰，易于控制。

## 正文
Promise是一个接口。

它用来处理的对象具有这样的特点：在未来某一时刻（主要是异步调用）会从服务端返回或者被填充属性。其核心是，<b>Promise是一个带有then()函数的对象</b>。

为了展示它的优点，我们来看一个例子，其中需要获取用户当前的配置文件：

```
var currentProfile = null;
var username = 'something';

fetchServerConfig(function(serverConfig){
	fetchUserProfiles(serverConfig.USER_PROFILES, username, function(profiles){
		currentProfiles = profiles.currentProfile;
	});
});
```

上面这种处理方式存在一些问题：
+ 1.对于代码缩进来说，这种代码就是一个噩梦，尤其在你需要链式调用很多次的时候。
+ 2.处于回调和函数之间的错误报告非常容易丢失，除非你在每一个步骤中都手动处理错误。
+ 3.如果需要使用currentProfile对象来做一些事情，那么你需要在最内层的回调中封装真正想要实现的逻辑，要么直接封装，要么通过一个单独的函数来封装。

promise机制可以很好地解决这些问题。在深入了解其运行机制之前，我们来看看如何使用promise实现同样的事情：

```
var currentProfile = 
	fetchServerConfig().then(function(serverConfig){
		return fetchUserProfiles(serverConfig.USER_PROFILES, username);
	}).then(function(profiles){
		return profiles.currentProfile;
	}, function(error){
		//可以在这里处理错误，在fetchServerConfig或者fetchUserProfiles中处理都可以
	});
```

使用promise机制的优点如下：
+ 1.可以对函数进行链式调用，所以你不会陷入代码缩进噩梦中。
+ 2.在调用链的过程中，可以保证上一个函数调用完成之后才会调用下一个函数。
+ 3.每一个then()调用都带有两个参数（两个都是函数）。第一个是成功之后回调，第二个是出错之后的处理器。
+ 4.如果调用链中出现了错误，错误将会被冒泡传递到其余的错误处理函数中。所以，最终来说，所有错误都可以在任意一个回调函数中进行处理。

你可能会额外呢，resolve（解决）和reject（拒绝）方法又是什么呢？在AngularJS中，延迟调用是实现promise的一种方式。调用resolve方法将会填充promise（也就是调用success处理函数），而调用reject方法将会调用promise的错误处理函数。
