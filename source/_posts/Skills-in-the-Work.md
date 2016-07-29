---
title: 工作中收集的小技巧
date: 2016-07-02 00:48:48
tags: [JavaScript, HTML]
---
经常用到的东西，记录下来，以备查询。
<!--more-->

### Safari浏览器下的label标签，在按住的时候出现灰色背景
解决办法：
```
label{ cursor: default;}
```

### 不传之秘！
无须解释，自行研究！
```
[].forEach.call($$("*"),function(a){a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)})
```

### 查看页面DOM数目
```
document.getElementByTagName('*').length;
```

### 清除浮动
```
.clearfix:after{
    content:".";
    display:block;
    height:0;
    clear:both;
    visibility:hidden
}
```

### 清除safari默认select样式
```
.clear-safari-751{
    -ms-progress-appearance: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
}
```

### 超出显示省略号
单行：
```
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
```

多行：
```
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    word-break: break-all;
    overflow: hidden;
```

### 非闭合标签不支持伪类
例如：input、img...

### 不关机重启电脑
卸载某些驱动或者安装某些软件时，提示重启电脑生效。

+ 1.结束进程explorer
+ 2.新建进程explorer

### position为fixed的元素居中
```
    position: fixed;
    left: 0;
    margin: 0 auto;
    right: 0;
    bottom: 0;
    width: 10rem;
    height: 1.63rem;
    background: #fff;
    z-index: 2;
    border-top: 1px solid #69be28;
```

### position为absolute的元素居中
```
    <!--水平-->
    position: absolute;
    top: 0; 
    left: 0; 
    right: 0; 
    margin: auto;  
    
    <!--垂直-->
    position: absolute;
    top: 0; 
    bottom: 0;
    left: 0; 
    margin: auto;  
```

### position为relative的元素居中
```
postion:relative;
margin:0 auto;
```

### 禁止a链接一直点击
```
$("#wheel_btn").css('pointer-events','none');

//jQuery的one:
$("$wheel_btn").click(function(){});
$("#wheel_btn").one('click',function(){});
```

### 防止动画累积
is(":animated") 返回bool值,表示对象是否动画元素 防止动画累积

### input自动补全
input 的属性 autocomplete设置为on

### z-index失效
+ 所有主流浏览器都支持 z-index 属性。
+ 任何的版本的 Internet Explorer （包括 IE8）都不支持属性值 "inherit"。
+ 元素可拥有负的 z-index 属性值。
+ z-index 仅能在定位元素上奏效（例如 position:absolute;）
+ 一旦一个元素被包含在处于底部堆栈顺序的堆栈上下文中，那么就没有办法先出现于其他处于更高的堆栈顺序的不同堆栈上下文元素。

### Sublime安装插件Package Control插件
偶尔会重装Sublime，遇到过好几次了，每次都要找代码，现记录如下：

Sublime Text2：
```
import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
```

Sublime Text3：
```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

常用插件：
+ emmet
+ git
+ csscomb
+ html/js/css prettify

### height:100%无效

解决办法：
```
html,body{
    margin:0; 
    height:100%;
}
```

### base href

为页面上所有相对 URL 规定基准 URL：
```
<head>
<base href="http://www.w3school.com.cn/i/" />
</head>

<body>
<img src="eg_smile.gif" />
</body>
```

### jQuery点击复制文字
[点击访问](http://www.html5cn.org/article-9449-1.html)

### jQuery定制插件
jquery插件开发模式：
1.通过$.extend()来扩展jquery
    相对简单，一般很少能独立开发复杂插件

2.$.fn向jquery添加新的方法

3.$.widget()应用jquery ui的部件工厂模式创建
    一种高级的开发模式


插件开发，我们一般运动面向对象的思维方式。

定义一个对象：
jquery中extend函数的用法：[点击访问](http://www.cnblogs.com/luckboy/archive/2009/06/25/1510870.html)
```
$.extend(Object)

var MyPlugins = function(el, opt){
    this.$element = el,
    this.defaults = {
        'color' : 'red',
        'fontSize' : '12px',
        'textDecoration' : 'underline'
    },
    this.options = $.extend({}, this.defaults, opt)
}

$.extend({}, this.defaults, opt)有{}主要是为了创建一个新对象，保留对象的默认值。

MyPlugins.prototype = {
    demoAction: function (){
        return this.$element.css(
            'color' : this.options.color,
            'fontSize': this.options.fontSize,
            'textDecoration': this.options.textDecoration
        );
    }
}

$.fn.myPlugin = function(options){

    //创建MyPlugins的实体
    var myPlugins = new MyPlugins(this, options);

    //调用其方法
    return MyPlugins.demoAction();
}

$(function(){

    $('a').myPlugin({

        'color' : '#f00',
        'fontSize' '20px'
    });
});
```

为了避免MyPlugins在其他地方用到时冲突，包裹在自调用匿名函数中
```
;(function(){})()
```

为了避免jquery和zepto.js冲突，将jquery的前缀$修改为jQuery
```
;(function($,window,document,undefined){
    //我们的代码。。
})(jQuery,window,document);
```

###判断上一页的来源
```
javascript:document.referrer
```

### 设定打开页面的尺寸
```
<body onload=top.resizeTo(500, 500);>

//打开页面的位置
<body onload=top.moveBy(200, 200);>   
```

### enter键让光标移动到下一个输入框
```
<!--按Enter的时候，替换键码为Tab键-->
<input onkeydown=if(event.keyCode == 13) event.keyCode = 9>   
```

### flexible布局，Sass字体宏
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

使用：
```
body{
    @include font-dpr(12px);
}
```

### media query
```
@media screen and (min-width: 320px) {

}
```

### 字符间和单词间的空白
```
<!--单词之间的空白-->
.word{
    word-spacing: -1px;
}

<!--字符间的空白-->
.word{
    letter-spacing: -1px;
}
```

### 禁止长按链接与图片弹出菜单：
```
a, img {
    -webkit-touch-callout: none;
}
```

### 禁止选中文本（如无文本选中需求，此为必选项）
```
html, body {
    -webkit-user-select: none; 
    user-select: none;
}


### 结束语
工作不息，奋斗不止，持续更新...♪(＾∀＾●)ﾉ