---
title: 如何自定义checkbox？
date: 2016-07-11 20:26:41
tags: [html, css]
---

自定义checkbox；善用after伪类。
<!--more-->

## 正文
[在线预览](http://7xtoaz.com1.z0.glb.clouddn.com/checkboxDemo.html)

### 思路
+ 1.隐藏原生的checkbox。
+ 2.添加label标签，通过for属性指向被隐藏checkbox，已达到checkbox本身复选的功能。
+ 3.通过after伪类及修改包裹checkbox的容器的样式，达到各种想要的效果。

### 代码

HTML:
```
<div class="checkbox1">
    <input type="checkbox" value="0" id="checkbox1" name="">
    <label for="checkbox1"></label>
</div>
```
CSS:
```
.checkbox1
{
    position: relative;

    width: 18px;
    height: 18px;

    border-radius: 100%;
}
.checkbox1 input[type=checkbox]
{
    visibility: hidden;
}
.checkbox1 label
{
    position: absolute;
    z-index: 1;
    top: 0;
    left: 0;

    display: block;

    width: 18px;
    height: 18px;

    cursor: pointer;
    -webkit-transition: all .5s ease;
       -moz-transition: all .5s ease;
        -ms-transition: all .5s ease;
         -o-transition: all .5s ease;
            transition: all .5s ease;

    border: 1px solid #c8c8c8;
    border-radius: 100px;
}

.checkbox1 input[type=checkbox]:checked + label:after
{
    position: absolute;
    top: 5px;
    left: 3px;

    width: 10px;
    height: 4px;

    content: '';
    -webkit-transform: rotate(-45deg);
       -moz-transform: rotate(-45deg);
        -ms-transform: rotate(-45deg);
         -o-transform: rotate(-45deg);
            transform: rotate(-45deg);

    opacity: 1;
    border: 2px solid #fff;
    border-top: none;
    border-right: none;
    background: transparent;
}
.checkbox1 input[type=checkbox]:checked + label
{
    border: 1px solid #26ca28;
    background: #26ca28;
}
```

## End
今日，突然发现很多之间的博文留下的坑没有填，这不是个好现象啊，要不订个计划每个月末专门填坑？
