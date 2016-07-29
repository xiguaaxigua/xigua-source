---
title: 关于CSS的思考
date: 2016-07-26 19:55:28
tags: [CSS]
---

CSS命名应该最简单、最直接，直捣黄龙。
<!-- more -->

## 当前遇到的问题
在命名CSS的时候掺杂语义，这样可以使代码的可读性更高，随之而来的问题是：
+ 浪费脑细胞，经常为了某个命名纠结半天；
+ 重用性很低，事实上语义越强烈的命名越没有重用性；
+ 命名冲突，个人的习惯直接决定了CSS的命名，一不留神就各种冲突；
+ 不利于团队协作，不得不说，同事们需要百度翻译才能理解我写的CSS是干嘛的。

另外一个很弱智的问题是：当前项目使用SASS，由于个人经验不足，自以为是的各种嵌套，命名冲突解决了，随之而来的是更加致命的性能问题。很丢人的一件事情是今天才搞明白CSS的渲染机制。

## 浏览器是怎么渲染页面的

### 浏览器的上层结构
浏览器的主要概念：
+ 用户接口 – 包括地址栏，前进后退，书签菜单等窗口上除了网页显示区域以外的部分。
+ 浏览器引擎 – 查询与操作渲染引擎的接口。
+ 渲染引擎 – 负责显示请求的内容。比如请求到HTML, 它会负责解析HTML 与 CSS 并将结果显示到窗口中。
+ 网络 – 用于网络请求, 如HTTP请求。它包括平台无关的接口和各平台独立的实现。
+ UI后端 – 绘制基础元件，如组合框与窗口。它提供平台无关的接口，内部使用操作系统的相应实现。
+ JavaScript解释器。用于解析执行JavaScript代码。
+ 数据存储。这是一个持久层。浏览器需要把所有数据存到硬盘上，如cookies。新的HTML规范 (HTML5) 规定了一个完整（虽然轻量级）的浏览器中的数据库：’web database’。

与其它浏览器不同，chrome使用多个渲染引擎实例，每个Tab一个，每个Tab都是一个独立进程。

### 渲染引擎
常见的渲染引擎：
+ Trident渲染引擎，使用该渲染引擎的浏览器有：
	+ IE
	+ 傲游
	+ 世界之窗浏览器
	+ 还有很多我没见过、没用过的浏览器...
+ Gecko渲染引擎，使用该渲染引擎的浏览器有：
	+ Firefox
	+ 更多我没见过、没用过的浏览器...
+ Webkit渲染引擎，使用该渲染引擎的浏览器有：
	+ Chrome
	+ Safari
	+ 没见过、没用过，你懂得...
+ Presto渲染引擎，使用该渲染引擎的浏览器有：
	+ Opera
	+ 任天堂DS浏览器，啊？任天堂也有浏览器？
+ Tasman渲染引擎，Mac中的IE浏览器使用该引擎，有Mac谁还有IE啊，真逗。
+ 还有一些其他的。

基本的渲染过程如下图：（图是偷的，我想没有人会在意的）
![](http://7xtoaz.com1.z0.glb.clouddn.com/Thinking-in-CSS1.png)

+ 1.渲染引擎首先解析HTML，生成一颗DOM树；
+ 2.浏览器解析样式表，和HTML中的显示控制一起生成一颗渲染树；
+ 3.渲染树完成之后进行布局，确定每个节点在页面中的具体位置；
+ 4.绘制渲染树，遍历渲染树，并用UI后端层将每一个节点绘制出来。

上面只是渲染引擎一般的渲染过程，不同渲染引擎的具体步骤有所不同。

Webkit渲染引擎的渲染过程：
![](http://7xtoaz.com1.z0.glb.clouddn.com/Thinking-in-CSS2.png)

可以看到，Webkit同时解析HTML和CSS，将HTML解析为DOM树，将CSS解析为“样式规则（Styles Rules）”（可能是为了加快生成渲染树），渲染树的生成和渲染树的布局也是同时的，貌似可以理解为生成一个布局一个，最后绘制渲染树，显示页面。

Gecko渲染引擎的渲染过程：
![](http://7xtoaz.com1.z0.glb.clouddn.com/Thinking-in-CSS3.png)

Gecko 里把格式化好的可视元素称做“帧树”（Frame tree）。每个元素就是一个帧（frame）。 Webkit 则使用”渲染树”这个术语，渲染树由”渲染对象”组成。Webkit 里使用”layout”表示元素的布局，Gecko则称为”Reflow”。Webkit使用”Attachment”来连接DOM节点与可视化信息以构建渲染树。一个非语义上的小差别是Gecko在HTML与DOM树之间有一个附加的层 ，称作”content sink”，是创建DOM对象的工厂。

此处，我还想再深入一点，马克。

## CSS的匹配规则

随便写一点代码：

```
.mod-div h3 span{
	//样式
}
```

天真的我一直傻乎乎的以为浏览器解析CSS的规则是：先找class为mod-div的元素，再在当前找到的元素中找h3，再找span元素。这样的话，就像定位一样，先找到你是哪个省的，再找你是哪个市的，再找你。

然而，并不是！而且从右到左比从左到右更加高效！

CSS选择器的正确解析姿势是从右到左，先找到所有的span元素，然后沿着span的父元素查找h3，中途找到了符合规则的节点就加入结果集，如果直到根元素html都没有找到，则不再遍历这条路径，从下一个span元素开始重复这个过程。

上述代码中的 `.mod-div h3 span` 构成一条索引树，树由上至下的节点是规则中从右到左的一个个选择符匹配的节点。
![](http://7xtoaz.com1.z0.glb.clouddn.com/Thinking-in-CSS4.png)

假如DOM结构如上图，匹配规则是 `.mod-div h3 span` 。

若从左到右匹配，过程是：

从.mod-nav开始，遍历子节点header和子节点div，然后各自向子节点遍历。在右侧div的分支中，最后遍历到叶子节点a，发现不符合规则，需要回溯到ul节点，再遍历下一个li-a，假如有1000个li，则这1000次的遍历与回溯会损失很多性能。

若从右到左匹配，过程是：

先找到所有的最右节点span，对于每一个span，向上寻找节点h3，由h3再向上寻找class=mod-nav的节点，最后找到根元素html则结束这个分支的遍历。

好吧，这下子我承认是我low逼，从右到左果然可以提高性能，也就是说，一定要减少选择器的嵌套层次！

这也就解释了这种写法的原因，能用子选择器就不要用后代选择器。

```
// 优
#test > .test{
	color: #000;
}

// 劣
#test .test{
	color: #000;
}
```



## 简单的看一下巨头们的页面
淘宝：
```
.sea-fp-discover .module-wrap .discover-wrapper .discover .discover-item .item-desc h4{
	margin-bottom: 12px;
    font-weight: 100;
    line-height: 13px;
}
```

Excuse me？比我还狠，嵌套七层！数年的维护，hack成对，淘宝的页面果然不太适合教学。

京东：
```
.activity-list {
	border-bottom:8px;
	padding:0 0 0 9px;
}
.jd-auto-complete-list li,.jd-header-bar {
	min-height:44px;
	border-bottom:1px solid #bfbfbf;
}
.activity-list li {
	display:block;
	margin:0;
	padding:0;
	background:#fff;
	line-height:0;
	margin-top:8px;
}
.activity-list li a {
	display:block;
	width:auto;
	padding:0;
	margin:0;
	margin-right:8px;
}
```

个人感觉京东的CSS写的很简洁，基本都是一层或两层嵌套，很少超过三成的。语义也很强，阅读起来很顺畅。但这样命名冲突应该是首当其冲的问题吧，真的很想知道京东具体是怎么解决的！

Facebook:
```
._8o,._8o .img{display:block}._8r{margin-right:5px}._8s{margin-right:8px}._8t{margin-right:10px}
._6a{display:inline-block}._6d{vertical-align:bottom}._6b{vertical-align:middle}._6e{vertical-align:top}._5u5j{width:100%}
._4bl7{float:left;min-height:1px}._4bl8{float:right}._4bl9{overflow:hidden}._4bl7,._4bl9{word-wrap:break-word}
._ohe{float:left}._ohf{float:right}
```

一个大写的服！果然精简直接，一看就知道CSS这块绝逼不会拖慢网页的性能，可惜我看不懂，或许他们有一套自己的规则，又或许，我的水平之低还不足以发现FB的美。

## “原子类”
当前项目中并不适用，留坑待填。

## 关于书写CSS的建议
+ 1.避免使用CSS表达式；
+ 2.能缩写尽量缩写；
	```
	.test{
		color: #000000;
		margin-top: 10px;
		margin-bottom: 20px;
		margin-left: 30px;

		font-style: italic; 
		font-variant: small-caps; 
		font-weight: bold; 
		font-size: 1em; 
		fine-height: 140%; 
		font-family: sans-serif;

		background-color: #f00; 
		background-image: url(background.gif); 
		background-repeat: no-repeat; 
		background-attachment: fixed; 
		background-position: 0 0; 
	}
	// 简写为：
	.text{
		color: #000;
		margin: 10px 0 20px 30px;
		font: italic small-caps bold 1em 140% sans-serief; 
		background: #f00 url(background.gif) no-repeat fixed 0 0;
	}
	```
+ 3.避免使用*匹配符。
+ 4.避免使用通用规则，查找页面上的所有节点，如果有节点存在“hidden”属性，并且其属性值为“true”，则匹配成功。这是最耗时耗力的匹配，页面上的所有节点都需要进行匹配运算。
	```
	[hidden="true"] {
		// 样式
	}
	```
+ 5.不要在class前再加上标签的名称，这样一点都不高雅，只会更慢。
	```
	// 劣
	button.btn{
		background: #fff;
	}

	// 优
	.btn{
		background: #fff;
	}
	```
+ 6.减少嵌套层次。
+ 7.避免使用后代选择器，尽量用子选择器替代。
	```
	// 劣
	h1 a{
		color: #000;
	}
	// 优
	h1 > a{
		color: #000;
	}
	```
+ 8.合理使用CSS的继承机制。
+ 9.避免过分重排（浏览器重新计算布局位置与大小），常见的重排元素：
	+ width、height、min-height；
	+ padding、margin；
	+ top、right、bottom、left；
	+ display；
	+ postion、float；
	+ font-size、font-family、line-height、text-align、vertical-align；
	+ clear、white-space、overflow、overflow-y。
+ 10.避免过分的重绘，常见的重绘元素：
	+ color、border-style、visibility；
	+ background、background-image、background-postion、background-repeat、background-size；
	+ outline、outline-color、outline-style、outline-width。
	+ border-radius、box-shadow。

+ 10.慎重选择高性能的样式。高性能属性：
	+ box-shadow；
	+ border-radius；
	+ transforms、transparency；
	+ CSS filters（性能杀手）。

## 相关文章
+ [各种浏览器的页面渲染引擎简介](http://blog.csdn.net/wei_ge163/article/details/7631112)
+ [How browsers work](http://taligarsiel.com/Projects/howbrowserswork1.htm)
+ [浏览器是怎样工作的（一）：基础知识](http://ued.ctrip.com/blog/how-browsers-work-i-basic-knowledge.html)
+ [浏览器是怎样工作的：渲染引擎，HTML解析（连载二）](http://ued.ctrip.com/blog/how-browsers-work-rendering-engine-html-parsing-series-ii.html)
+ [css 原理及其优化](http://www.cnblogs.com/web-ed2/archive/2011/08/03/2126748.html)
+ [精简高效的CSS命名准则/方法](http://www.jiangweishan.com/article/%E7%B2%BE%E7%AE%80%E9%AB%98%E6%95%88%E7%9A%84CSS%E5%91%BD%E5%90%8D%E5%87%86%E5%88%99%2F%E6%96%B9%E6%B3%95.html)
+ [我是如何对网站CSS进行架构的](http://www.zhangxinxu.com/wordpress/2010/07/%E6%88%91%E6%98%AF%E5%A6%82%E4%BD%95%E5%AF%B9%E7%BD%91%E7%AB%99css%E8%BF%9B%E8%A1%8C%E6%9E%B6%E6%9E%84%E7%9A%84/)
+ [W3C中文](http://w3help.org/zh-cn/standards/)
+ [视频: 浏览器的背后](http://v.youku.com/v_show/id_XMjMzMzU2NDc2.html)
+ [提高 web 应用性能之 CSS 性能调优](http://www.ibm.com/developerworks/cn/web/1109_zhouxiang_optcss/)
+ [W3C-CSS标准](https://www.w3.org/TR/CSS21/)
+ [谈谈CSS性能](https://www.w3.org/2015/Talks/0109-CSSConf-xq/)

## End
个人还是倾向于京东的做法，够简单够直接，嵌套层数少，渲染效率高，唯一的问题是命名不太容易。于是乎，我是这么干的：
+ 1.每个页面的所有内容都包含在一个特定的div中；
+ 2.在该div下保证所有的命名不冲突，这样嵌套也就两三层，算是半解决了当前的问题吧。

例如这样：
```
// index.html文件的scss代码
.index-box751{ // 751是我的工号，避免和其他同事冲突，

	// index-box751中不会再出现XXX-box751类型的样式，避免冲突
	.banner{
		// 样式
	}
	.banner-content{ // 讲道理的话，应该嵌套在.banner751中，但是为了减少嵌套，单独拿出来
		// 样式
	}
	.user-login{
		// 样式
	}
	.user-login-tips{
		// 样式
	}
	.confirm-terms{
		// 样式
	}
}
```

写完了发现，我并没有从根本上解决问题，只是减轻了命名冲突的难度，减少了嵌套的层数，保留了语义化的命名，增加了没有必要的DOM，虽然只有一个。