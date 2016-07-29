---
title: Flex伸缩布局
date: 2016-07-21 20:17:04
tags: [HTML, CSS]
---

一篇文章搞懂伸缩布局。
<!-- more -->

![](http://7xtoaz.com1.z0.glb.clouddn.com/flex1.png)

## 写在前面
网页布局是HTML+CSS中重要的一部分。是前端开发的基本功，对各种布局模式了熟于心，才能“下笔如有神”。

常见的网页布局模式有哪些？
+ 块布局，为了呈现文档而设计出来的布局模式；
+ 行内布局，为了呈现文本而设计出来的布局模式；
+ 表格布局，为了用格子呈现2D数据而设计出来的布局模式；
+ 定位布局，为了非常直接的定位元素而设计出来的布局模式，定位元素基本与其他元素无关；
+ 伸缩布局，为了呈现复杂的应用与页面而设计出来的布局模式。

其中，前四种布局模式是CSS2.1定义的，第五个布局模式—伸缩布局，是2009年W3C提出的新的布局模式，目前已经得到了所有浏览器的支持，但是需要写hack。

浏览器支持情况：
+ Chrome21+
+ Opera12.1+
+ Firefox22+
+ Safari6.1+
+ IE10+（坑爹的IE！这意味着目前来说，在移动端还好，在PC站使用伸缩布局还是算了吧）

盒状模型布局模式应该是前端开发的入门课程吧，基本上所有的页面都可以通过盒状模型来完成。但在垂直居中等特殊场景下，很不容易实现。

博主写下这篇文章一是为了记录自己的学习过程，便于以后翻阅；二是希望某一天能帮助到别人。这也正是这个博客的目的所在。

部分内容翻译自W3C官方对伸缩布局的介绍。

## 什么是伸缩布局？
伸缩布局与块布局十分类似，伸缩布局不具有浮动、多栏等在块布局中使用的复杂、面向文本/文档的属性，而是通过简单、强大的空间分配和内容对其的工具，可以用来处理复杂的页面。

伸缩容器具有以下特点：
+ 可以沿任何方向布局，当然只是上下左右；
+ 最终实现效果和样式的书写顺序有关；
+ 可以沿着两条相互垂直的轴线配置，分别为主轴和侧轴；
+ 可以根据可用空间调整伸缩容器的尺寸；
+ 可以沿着容器或者相互对齐；
+ 可以在保持侧轴长度不变的时候动态折叠或者反折叠。

一个设置了`display: flex`或者`display: inline-flex`的元素称为：伸缩容器，伸缩容器的子元素称为：伸缩项目。这些子元素可以使用伸缩布局模型来排版。

块布局模式中，偏向于使用书写模式的方向作为布局的方向，伸缩布局则偏向于使用伸缩流方向。下边这张图很好的解释了主轴、侧轴的概念。
![](http://7xtoaz.com1.z0.glb.clouddn.com/flex2.svg)

掌握这些概念对理解伸缩布局很重要：
+ 主轴：默认是纵向的；
+ 侧轴：默认是横向的，与主轴垂直；
+ 主轴方向：默认从上到下，上边为主轴起点，下边为主轴终点；
+ 侧轴方向：默认从左到右，左边为侧轴起点，右边为侧轴终点；
+ 主轴侧轴的长度由width还是由height来确定，取决于哪一个对着轴的方向。
+ 主轴侧轴的方向可以互相调换；
+ 主轴侧轴的概念很像初中学习的坐标系中的象限。

## 语法篇

### 定义一个伸缩容器
```
.box{
	display: flex;
	width: 400px;
	height: 100px;
	background: yellow;
}
```
display的值有：block、inline、inline-block、inherit。如今又新增了flex和inline-flex。
+ display:flex 将元素变为伸缩容器
+ display:inline-flex 将元素变为行内级伸缩容器

伸缩容器会为其内容创建新的<b>伸缩格式上下文</b>。除了使用伸缩排版而不是用块排版外，伸缩格式化上下文与块格式化上下文根元素相同——浮动不会闯入伸缩容器，且伸缩容器不与其内容的边界折叠。这段话看似很拗口，其实意思很简单：使用伸缩布局后，以往使用块布局的属性将会失效。例如：
+ 多栏布局中的column-*属性将会失效；
+ float和clear属性将会失效；
+ vertical-align属性将会失效；

### 伸缩容器的属性
共有6个属性可以定义在伸缩容器上：
+ flex-direction
+ flex-wrap
+ justify-content
+ align-items
+ align-content

#### flex-direction
flex-direction决定了主轴的方向。其值有四种：
+ row（默认值），从左到右；
+ row-reverse，从右到左；
+ column，从上到下；
+ column-reverse，从下到上。

语法结构：
```
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

![](http://7xtoaz.com1.z0.glb.clouddn.com/flex3.png)

#### flex-wrap
默认情况下，项目都排在同一条线上，flex-wrap决定如果一条轴线装不下的时候，如何换行。

语法结构：
```
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

![](http://7xtoaz.com1.z0.glb.clouddn.com/flex4.png)

其值有三种：
1.nowrap，不换行；
![](http://7xtoaz.com1.z0.glb.clouddn.com/flex5.png)

2.wrap，换行，第一行在上方；
![](http://7xtoaz.com1.z0.glb.clouddn.com/flex6.jpg)

3.wrap-reverse，换行，第一行在下方。
![](http://7xtoaz.com1.z0.glb.clouddn.com/flex7.jpg)

#### flex-flow
flex-flow是flex-direction和flex-wrap的简写形式。默认值为：`row nowrap`。

语法结构：
```
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

#### justify-content
justify-content决定了项目在主轴上的对其方式。其值有五种：
+ flex-start（默认值），起点对齐；
+ flex-end，终点对齐；
+ center，居中；
+ space-between，两端对齐，伸缩项目之间的间隔相等；
+ space-around，每个项目两侧的间隔相等。即，项目之间的间隔比项目与边框的间隔大一倍。

语法结构：
```
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

![](http://7xtoaz.com1.z0.glb.clouddn.com/flex8.png)

#### align-items
align-items决定了项目在侧轴上的对其方式。其值有五种：
+ flex-start，起点对齐；
+ flex-end，终点对齐；
+ center，居中；
+ baseline，项目的第一行文字的基线对齐；
+ stretch（默认值），如果项目未设置高度或者设置为auto，将占满整个屏幕。

语法结构：
```
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

![](http://7xtoaz.com1.z0.glb.clouddn.com/flex9.png)

#### align-content
align-content决定了多根轴线的对齐方式。如果项目只有一根轴线，则该属性无效。其值有六种：
+ flex-start，与交叉轴的起点对齐；
+ flex-end，与交叉轴的终点对齐；
+ center，居中；
+ space-between，两端对齐，伸缩项目之间的间隔相等；
+ space-around，每个项目两侧的间隔相等。即，项目之间的间隔比项目与边框的间隔大一倍。
+ stretch（默认值），轴线占满整个交叉轴。

语法结构：
```
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

![](http://7xtoaz.com1.z0.glb.clouddn.com/flex13.png)

### 定义一个伸缩项目
```
.item{
	flex: 1;
	width: 10px;
	border-radius: 50%;
	background: black;
}
```

一个伸缩容器的内容具有零个以上的伸缩项目——伸缩容器的每个子元素都会称为一个伸缩项目，并且伸缩容器的连续文字块将视为无名伸缩项目，但是如果无名伸缩项目为空白，将不会渲染该伸缩项目，等价于display:none。

### 伸缩项目的属性
共有6个属性可以定义在伸缩容器上：
+ order
+ flex-grow
+ flex-shrink
+ flex-basis
+ align-self

#### order
order决定了项目的排列顺序。值为数字，默认为0，数字越小，排序越靠前。

语法结构：
```
.item {
  order: <integer>;
}
```

![](http://7xtoaz.com1.z0.glb.clouddn.com/flex10.png)

#### flex-grow
flex-grow决定了项目的放大比例。值为数字，默认为0，即如果存在剩余空间，也不放大。与flex-shrink相反。
如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

语法结构：
```
.item {
  flex-grow: <number>; /* default 0 */
}
```

![](http://7xtoaz.com1.z0.glb.clouddn.com/flex11.png)

#### flex-shrink
flex-shrink决定了项目的多小比例。值为数字，默认为0，即如果剩余空间不足，也不缩小。与flex-grow相反。<b>负值对该属性无效。</b>

语法结构：
```
.item {
  flex-shrink: <number>; /* default 1 */
}
```

如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
![](http://7xtoaz.com1.z0.glb.clouddn.com/flex12.jpg)

#### flex-basis
flex-basis决定了在分配多余空间之前，伸缩项目占据的主轴空间。浏览器根据这个属性计算主轴是否有多余空间，默认值为auto，即伸缩项目原本的大小。

语法结构：
```
.item {
  flex-basis: <length> | auto; /* default auto */
}
```
它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。

#### flex
flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为`0 1 auto`。后两个属性可选。该属性有两个快捷值：`auto(1 1 auto)` 和 `none (0 0 auto`，建议优先使用这个属性。

语法结构：
```
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

#### align-self
align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

语法结构：
```
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

![](http://7xtoaz.com1.z0.glb.clouddn.com/flex14.png)

### 布局篇


### 使用场景


### 相关文章
+ [W3C官方 - CSS 伸缩盒布局模组](https://www.w3.org/html/ig/zh/css-flex-1/)
+ [使用CSS3 Flexbox布局](http://www.w3cplus.com/css3/css3-flexbox-layout.html)
+ [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)
+ [Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)
+ [Using the CSS3 flexbox layout](http://helephant.com/2013/03/23/css3-flexbox-layout/)

## End
到点睡觉了，明日再补充。