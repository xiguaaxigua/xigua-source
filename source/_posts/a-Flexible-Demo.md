---
title: 一个使用Flexible实现H5布局的Demo
date: 2016-07-10 20:57:15
tags: [框架]
---
移动端可伸缩布局方案；兼容大部分机型。
<!--more-->

### Flexible布局方案是什么
我个人的理解是：

通过JavaScript动态的向html标签添加`font-size`属性（值即为1rem所代表的宽度），然后根据该基准对整个页面进行布局，从而达到适配大多数机型的布局方案。官方地址：[点击访问](https://github.com/amfe/lib-flexible)

### 设计图及效果图
UI给的设计图如下：
<img src="http://7xtoaz.com1.z0.glb.clouddn.com/Flexible1.jpg" style="width:30%" />

效果图如下：
<img src="http://7xtoaz.com1.z0.glb.clouddn.com/Flexible2.png" style="width:30%" />

注：效果图为锤子T1手机，其他机型基本全适配，当然也包括iPad哦。另外，由于只是Demo并未百分百还原设计。

### Flexible布局的相关文章
关于Flexible布局方案有一篇文章已经很简洁明了了，此处没有必要再做无用功。

+ [使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)
+ [深入了解viewport和px](http://tgideas.qq.com/webplat/info/news_version3/804/7104/7106/m5723/201509/376281.shtml)
+ [移动端适配方案(上) ](https://github.com/riskers/blog/issues/17)
+ [移动端适配方案(下) ](https://github.com/riskers/blog/issues/18)

### Flexible的核心
我们知道BootStrap会将页面横向分为12个栅格，Flexible很类似的将页面分为了10个rem。

下面这张图很直观的表达出了Flexible的核心：
<img src="http://7xtoaz.com1.z0.glb.clouddn.com/Flexible4.jpg" style="width:50%" />

### 关于字体
在使用Flexible布局的时候，切记对字体不要使用rem单位，切记切记！因为在网速不好时，页面没有完全加载完毕的时候，你会发现，字体可能一瞬间变的很大！

我们可以写个SASS的mixin：

```
@mixin font-dpr($font-size){
    font-size: $font-size;
    [data-dpr="2"] & {
        font-size: $font-size * 2;
    }
    [data-dpr="3"] & {
        font-size: $font-size * 3;
    }
}
```

在使用的时候姿势是这样的：
```
body{
    @include font-dpr(12px);
}
```

### iPad强行访问的时候
一般来说，iPad访问你的站的时候，会跳转到PC站，但是如果强行访问手机站的话，我们最起码不应该让页面乱了。

使用Flexible布局的时候可以这样处理，在容器最外部加个div，类似于下面这样：

```
.container{
    position: relative;
    margin: 0 auto;
    width: 10rem;
    border: 1px solid #000;
}
```

这样在iPad上的效果图大致是这样：
<img src="http://7xtoaz.com1.z0.glb.clouddn.com/Flexible3.jpg" style="width:30%" />

图为Chrome下的效果，真机上相差不大。

### 结束语
Flexible很轻巧，也很实用，也许以后会遇到一些问题，此处记录，留待以后填坑。