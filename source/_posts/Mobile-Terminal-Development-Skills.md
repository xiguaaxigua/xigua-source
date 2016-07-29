---
title: 移动端开发技巧
date: 2016-07-05 21:33:57
tags: [移动端]
---
腾讯Alloyteam团队开发规范；收集到的小技巧。
<!--more-->

## Part1.腾讯Alloyteam团队发布的移动开发规范
本文记录对该规范的理解，以及个人开发移动端的经验总结。[点击访问官网地址](http://alloyteam.github.io/Spirit/modules/Standard/index.html)

另外一篇很不错的文章，[移动前端系列——移动页面性能优化](http://tgideas.qq.com/webplat/info/news_version3/804/808/811/m579/201412/293834.shtml)。

### 字体设置
使用无衬线字体：
```
body {
    font-family: "Helvetica Neue", Helvetica, STHeiTi, sans-serif;
}

<!--
Helvetica Neue 、 Helvetica    ···Mac默认字体，iOS 4.0+使用Helvetica Neue
STHeiTi                        ···Mac中文：华文黑体STHeiTi
sans-serif                     ···网页常用无衬线字体
-->
```

TIP:
1.什么是无衬线字体？
衬线体（serif）和无衬线体（sans-serif）。像Times、Times New Roman等都是属于衬线体，而Arial、helvetica则是属于无衬线体。有衬线体和无衬线体从视觉上的区别如下：
![](http://7xtoaz.com1.z0.glb.clouddn.com/Mobile-Development-Skills.jpg)

可以通过这篇文章了解更多。[衬线体和无衬线体](https://www.douban.com/note/173404594/)

2.为什么要使用无衬线字体？
也许是视觉的原因，并不懂？

### 基础交互
设置全局的CSS样式，避免图中的长按弹出菜单与选中文本的行为

禁止长按链接与图片弹出菜单：
```
a, img {
    -webkit-touch-callout: none;
}
```

禁止选中文本（如无文本选中需求，此为必选项）
```
html, body {
    -webkit-user-select: none; 
    user-select: none;
}
```

### 移动性能
此部分各取所需。

要考虑Android低端机与2G网络场景下性能。

发布前必要检查项：
+ 所有图片必须有进行过压缩
+ 考虑适度的有损压缩，如转化为80%质量的jpg图片
+ 考虑把大图切成多张小图，常见在banner图过大的场景

加载性能优化, 达到打开足够快
+ 数据离线化，考虑将数据缓存在`localStorage`
+ 初始请求资源数`<4`
+ 图片使用`CSS Sprites`或`DataURI`
+ 外链`CSS`中避免`@import`引入
+ 考虑内嵌小型的静态资源内容
+ 初始请求资源gzip后总体积`<50kb`
+ 静态资源(HTML/CSS/JS/Image)是否优化压缩？
+ 避免打包大型类库
+ 确保接入层已开启Gzip压缩（考虑提升Gzip级别，使用CPU开销换取加载时间）
+ 尽量使用CSS3代替图片
+ 初始首屏之外的静态资源（JS/CSS）延迟加载 ！
+ 初始首屏之外的图片资源按需加载（判断可视区域）
+ 单页面应用(SPA)考虑延迟加载非首屏业务模块
+ 开启`Keep-Alive`链路复用

运行性能优化, 达到操作足够流畅
+ 避免`iOS 300+ms`点击延时问题
+ 缓存`DOM`选择与计算
+ 避免触发页面重绘的操作
+ Debounce连续触发的事件(scroll / resize / touchmove等)，避免高频繁触发执行
+ 尽可能使用事件代理，避免批量绑定事件
+ 使用CSS3动画代替JS动画
+ 避免在低端机上使用大量CSS3渐变阴影效果，可考虑降级效果来提升流畅度
+ HTML结构层级保持足够简单
+ 尽能少的使用CSS高级选择器与通配选择器
+ `Keep it simple`

在线性能检测评定工具使用指南
+ 访问[Google PageSpeed](http://developers.google.com/speed/pagespeed/insights/)在线评定网站
+ 在地址栏输入目标URL地址，点击分析按钮开始检测
+ 按`PageSpeed`分析出的建议进行优化，优先解决红色类别的问题

### 推荐阅读
+ [移动WEB性能](http://www.webperformancetoday.com/tag/mobile-web-performance/)
+ [优化WEB缓存](https://developers.google.com/speed/docs/best-practices/caching)
+ [最小化RTT次数](https://developers.google.com/speed/docs/best-practices/rtt)
+ [最小化请求负载](https://developers.google.com/speed/docs/best-practices/payload)
+ [优化浏览器渲染](https://developers.google.com/speed/docs/best-practices/rendering)

## Part2.经验总结
### webkit内核的meta标签
```
<!--强制让文档的宽度与设备的宽度保持1:1，并且文档最大的宽度比例是1.0，且不允许用户点击屏幕放大浏览-->
<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">

<!--iphone的私有标签，它表示：允许全屏模式浏览-->
<meta content="yes" name="apple-mobile-web-app-capable">

<!--iphone的私有标签，指定的iphone中safari顶端的状态条的样式-->
<meta content="black" name="apple-mobile-web-app-status-bar-style">

<!--告诉设备忽略将页面中的数字识别为电话号码-->
<meta content="telephone=no" name="format-detection">

<!--告诉安卓设备忽略识别邮件地址，iphone不识别-->
<meta content="email=no" name="format-detection" />
```

### 使用Zepto或者jQuery Mobile而不是jQuery或者jQuery-UI
+ jQuery Mobile相当于PC端的jQueryUI，提供了很多页面的UI库。
+ Zepto相当于PC端的jQuery。
+ Zepto.js是一个专为mobile WebKit浏览器(如：Safari和Chrome)而开发的一个JavaScript框架。轻量、简约，语法借鉴并兼容jQuery。

### 使用tap事件，而不是click。
在移动端请适当使用touchstart，touchend，touch等事件代替延迟比较大的click事件。Click之所以慢是因为mousedown导致的。
![](http://ossweb-img.qq.com/upload/webplat/info/tgideas/20141205/1417746127669833.jpg)

### 避免使用Float。
貌似会影响页面，建议使用inline-block。

### 块级化a标签
为了保证用户点击的范围足够大。

### 禁止自动识别电话和android自动识别邮箱
```
<meta content="telephone=no" name="format-detection" />
<meta content="email=no" name="format-detection" />
```

### 去除IOS和Android中的输入URL的控件条
```
$(function(){
    setTimeout(scrollTo,0,0,0);
})
```
使用条件：
+ 页面载入时就运行这段脚本。
+ 当前文档的内容高度高于窗口的高度

### 关闭IOS中键盘自动大写
```
<input type="text" autocapitalize="off">
```

### IOS中如何禁止用户选中文字
```
a{
    -webkit-user-select:none;
}
```

### IOS中如何禁止用户保存图片
```
img{
    -webkit-touch-callout:none;
}
```

### 解决IOS4.3版本中safari对页面中5位数字的自动识别和自动添加样式 
```
<button class="t-balance"style="background:none;padding:0;border:0;">95009.00</button>
```

### 手机拍照和上传图片
```
<input type=file accept="image/*">
<input type=file accept="video/*">
```

### 避免使用dataURI
dataURI在移动端并不如PC端吃香，而且速度比外链还慢。生成的代码文件相对图片文件体积没有减少反而增大，而且浏览器在对这种base64解码过程中需要消耗内存和cpu，这个在移动端坏处特别明显。

关于DataURI的几篇文章：
+ [DataURI详解](http://aiyouu.net/data-uris-explained/)
+ [Data URI的利弊](http://www.cssforest.org/blog/index.php?id=152)
+ [关于base64编码的原理及实现](http://www.cnblogs.com/hongru/archive/2012/01/14/2321397.html)
+ [手机端DataURI比外链资源慢6倍](http://www.qianduan.net/data-uris-are-slow-on-mobile.html)
+ [Data URIs explained](https://www.nczonline.net/blog/2009/10/27/data-uris-explained/)

### 针对渲染过程的优化
+ 动画优化
    + 尽量使用CSS3动画
        + CSS3动画不占JavaScript主线程
        + 可利用硬件加速
        + 浏览器可对动画做优化
        + 缺点：不支持中间状态监听
    + 适当使用canvas动画
    + 合理使用RAF
        + 能解决脚本问题引起的丢帧，卡顿问题
        + 支持中间状态监听
        + 缺点：兼容问题

结论：
+ 5个元素以内只用CSS3动画，超过使用canvas。
+ 不需要兼容android 4.3浏览器的情况下，使用RAF制作脚本动画