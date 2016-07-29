---
title: Angular的内置过滤器
date: 2016-05-20 10:33:00
tags: [Angular, 教程]
---
老外录制的Angular视频，学习笔记。
<!--more-->

## 视频

<video src="http://7xtoaz.com1.z0.glb.clouddn.com/8.Built%20in%20Filter.mp4" controls="true" width="100%"></video>

## 笔记

本例中的代码与上一篇[007-Angular中的nr-repeat](http://xiguaaxigua.cn/2016/05/20/Angular-Video-Tutorial-007/)相同，不再赘述。

### 现将Angular的内置过滤器总结如下：

+ 1.orderBy: 'name';
按照name字段的字母排序正序排列。

+ 2.orderBy: '-name';
按照name字段的字母排序逆序排列。

+ 3.limitTo: 5;
取前五条数据。

+ 4.limitTo: -5;
取后五条数据。

+ 7.lowercase;
将数据转换为小写。

+ 8.uppercase;
将数据转换为大写。

+ 9.多个过滤器可以协调使用。例如：
```
<tr ng-repeat="actor in avengers.cast | orderBy: 'name' | limitTo: -5 | lowercase"></tr>
```