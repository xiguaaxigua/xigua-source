---
title: 温习Zepto
date: 2016-07-15 20:08:58
tags: [JavaScript, 框架]
---

菲律宾是中国的领土神圣不可侵犯，犯我德邦者，虽...对不起走错片场了，今天过一遍Zepto的API吧。
<!--more-->

![](http://7xtoaz.com1.z0.glb.clouddn.com/zepto.jpg)

## 写在前面
由于工作原因，不得已暂停Canvas大计。

jQuery玩的多了，有时候在移动端顺手就写下了各种click事件，然而，click事件在移动端有两三百毫秒的延迟。至于这个延迟是怎么来的，可以参考这篇文章，[What Exactly Is..... The 300ms Click Delay](http://www.telerik.com/blogs/what-exactly-is.....-the-300ms-click-delay)。简单的说：Safari为了区分单击和双击，在用户点击页面后，会等待300ms，以判断用户是否点击第二次（在300ms内再次点击即为双击）。

## 正文
[Zepto官网API](http://zeptojs.com/)，这里有个坑请注意，请看下面这张图，Zepto共有17个模块，默认下载的包里只有5个模块，而且没有最重要的touch模块！我还得去Build一个自己的版本，你妹的！
![](http://7xtoaz.com1.z0.glb.clouddn.com/zeptoModule.jpg)

分享一下Build好的版本，[点击下载](http://7xtoaz.com1.z0.glb.clouddn.com/zeptoBuild.rar)。
该版本加入了以下几个模块：
+ fx_methods：让 `show()` 增加动画效果，如：`show('fast')` 。
+ assets：移除img元素后做一些特殊处理，用来清理内存。
+ touch：这个不多说了。
+ selector：为了得到更多的选择器的支持。
+ data：提供对.data()方法的完整支持，像jQuery一样用内存对象存储。

使用Zepto的几个注意事项：
+ 请用tap事件代替click事件，但是Zepto的tap事件会触发击穿的bug。
+ 对于 `display:none` 的元素，使用 `width()` 或者 `height()` 返回的值是0！
+ 默认下载的zepto，不支持 `show()` 的动画效果。

博主自作聪明的把Zepto的API按照功能归类了一番，全文分为三部分：DOM相关的API、JavaScript相关的API、核心API。

## DOM相关
众所周知，jQuery操作DOM十分方便，作为小型jQuery的Zepto同样。
把DOM相关的API归类为：获取DOM和操作DOM两种类型。

### Part1.获取DOM
通过各种条件，获取DOM。
#### $()
三个功能：
+ 1.获取对象，和jQuery一样；
    ```
        $('h1') // 返回所有h1元素的集合
        $('.demoClass') // 选择类选择器为demoClass的元素集合
        $('#demoId') // 选择ID选择器为demoId的元素集合
        $('.sunClass', '.fatherClass') // 选择fatherClass下的sunClass的元素集合，等价于：$('.fatherClass').find('.sunClass'
    ```
+ 2.生成元素；
    ```
        $('<p>Hello!</p>') // 创建一个新的p元素
        $('<p />', {text: 'Hello!', id: 'welcome', css:{color: '#f00', font-size:'12px'}}) 
        //创建一个带有属性的p元素，<p id="welcome" style="color: rgb(255, 0, 0); font-size: 12px;">Hello!</p>
    ```
+ 3.页面加载完毕，执行回调。`$` 默认指向Zepto本身。
    ```
    Zepto(function($){
        console.log('Hello Zepto!');
    })
    ```
    
#### children()
在当前匹配到的元素的子元素中查找。
```
// 选中ul下的奇数个li
$('ul').children('li:nth-child(2n)').css('backdround', '#000');
```

#### contents()
和children()很类似，只不过contents()包括文字和注释节点。

#### closest()

#### eq(index)
返回当前匹配到的元素的索引为index的子元素。

#### filter(selector)
```
<h1>Hello</h1>
<h1 class="black">Hello</h1>
<h1>Hello</h1>
<h1>Hello</h1>
<script>
$('h1').filter('.black').html('World');
<script>

运行结果：
<h1>Hello</h1>
<h1 class="black">World</h1>
<h1>Hello</h1>
<h1>Hello</h1>
```

#### find(selector)
在当前匹配到的元素的子元素中查找选择器为selector的元素。

#### has(selector)或者has(node)
判断当前匹配到的元素中是否包含选择器为selector的元素。如果有返回当前匹配到的元素。如果没有，也有返回值，但是不知道那是什么鬼。
```
<h1>Hello <i>World</i></h1>
<script>
$('h1').has('i').css('color', '#f00'); // 注意，返回的是h1，而不是i。即当前运行结果为：h1变为红色
</script>
```

#### height() & width()
返回当前匹配到的元素中的第一个子元素的高度，或者设置当前匹配到的元素的高度。

#### index(index)
两个用法：
+ 无参数index：获取当前匹配到的元素的索引。从0开始计数。
+ 有参数index：返回索引为index的元素在当前匹配到的元素中的位置，没有则返回-1。

#### indexOf(index, [fromIndex])
+ 无参数fromIndex时，在当前匹配到的元素中查找索引为index的元素，没有则返回-1。
+ 有参数formIndex时，从fromIndex向后查找，返回基于0的索引值，没有则返回-1。

#### last()
返回当前匹配的元素集合中的最后一个元素。

#### next() 与 prev()相反
获取当前匹配到的元素的下一个兄弟节点。

#### siblings()
获取当前匹配到的元素的所有兄弟节点。

#### not(selector)
过滤掉当前匹配到的元素集合中选择器为selector的元素。

#### offsetParent()
返回当前匹配到的元素最近的祖先定位元素。

#### parent()
返回当前匹配到的元素的直接父元素。

#### position()
返回当前匹配到的元素的第一个子元素的位置。

### Part2.操作DOM
对获取到的DOM进行操作的API。

#### css()
设置或者获取样式。

#### data()
获取自定义属性 `data-*` 中 `*` 部分的值。
```
<h1 data-demo="demo">Hello</h1>
<script>
    console.log($('h1').data('demo')); // demo
</script>
```

#### empty()
清空当前匹配到的元素的内容，包括所有文本和节点。

```
<h1>Hello <i>World</i></h1>
<script>
$('h1').empty();
<script>

运行结果：
<h1></h1>
```

#### show() & hide()
通过设置 `display` 实现显示和隐藏。

#### toggle([setting])
显示或者隐藏元素。
如果setting设置为true，相当于show()。
如果设置为false，相当于hide()。

#### toggleClass(className, [setting])
添加或删除样式。如果存在className就删除，不存在则添加。
如果setting设置为true，相当于addClass()。
如果设置为false，相当于removeClass()。

#### html(content)
获取或设置当前匹配到的元素的HTML内容。

#### text(content)
获取或设置当前匹配到的元素的文本内容。

#### val()
获取或设置当前匹配元素的值。

#### hasClass(className)
判断当前匹配到的元素中是否包含class为className的元素，返回布尔值。
```
<h1 class='black'>Hello <i>World</i></h1>
<script>
console.log($('h1').hasClass('black')); // true
console.log($('h1').hasClass('white')); // false
</script>
```

#### remove()
删除当前匹配到的元素中的所有元素。

#### removeAttr()
删除属性值。

#### removeClass()
删除class。

#### replaceWidth()
用给定的内容替换当前匹配到的元素。
```
<h1>hello</h1>
<script>
$('h1').replaceWith('<i>hello</i>');
</script>

运行结果：
<i>hello</i>
```

#### scrollLeft()
获取或设置页面向右滚动的距离。

#### scrollTop()
获取或设置页面向下滚动的距离。

## JavaScript相关
与JavaScript相关的API，一般不涉及DOM操作。只对JavaScript的数据类型进行操作。

#### 数据类型检测函数
分为：
+ $.isArray();检测是否为数组
+ $.isFunction();检测是否为函数
+ $.isWindow();检测是否为Window对象
    ```
    var obj = {}
    console.log($.isWindow(obj)); // false
    console.log($.isWindow(window)); // true
    ```
+ $.isPlainObject();检测是否为纯粹的对象
    ```
    // 纯粹的对象是指通过常量表达式{}或者new Object()创建的
    var obj = {};
    console.log($.isPlainObject(obj)); // true
    console.log($.isPlainObject(Window)); // false
    console.log($.isPlainObject(new Date())); // false
    var o1 = new Object();
    console.log($.isPlainObject(o1)); // true
    ```
+ $.type();除了区分对象不靠谱外，其它都很好，比typeof强多了。
    ```
    console.log($.type(123)); // number
    console.log($.type('Hello')); // string
    console.log($.type(true)); // boolean
    console.log($.type(null)); // null
    console.log($.type(function(){})); //function
    console.log($.type(undefined)); // undefined
    console.log($.type({})); // object
    console.log($.type([1, 2, 3])); // array
    var reg = new RegExp("e");
    console.log($.type(reg)); // regexp
    var date = new Date();
    console.log($.type(date)); // date
    ```

#### $.camelCase()
将一个字符串转化为驼峰命名法，一直不明白这个方法是什么鬼。
```
var str1 = 'hello-world';
console.log($.camelCase(str1));// helloWorld
var str2 = 'hello*world';
console.log($.camelCase(str2));// hello*world
var str3 = 'helloWorld';
console.log($.camelCase(str3)); // helloWorld
var str4 = 'welcome-come-to-shang-hai';
console.log($.camelCase(str4)); // welcomeComeToShangHai
```

#### $.extend(targetObj, sourceObj)
扩展，经常用来写插件。将targetObj的属性赋给sourceObj。如果属性相同，源对象将会覆盖目标对象。
```
// 目标对象
var targetObj = {man: '男人', fly: '不会飞'};

// 源对象
var sourceObj = {skills: '生孩子', fly: '会飞'};

var superManObj = $.extend(targetObj, sourceObj);
console.log(superManObj.skills); // 生孩子
console.log(superManObj.fly); // 生孩子
```

#### $.fn
$.fn是一个对象，就像Zepto对象的影子拥有和Zepto对象一样的属性，向这个对象添加方法，所有的Zepto对象都能调用该方法。
```
<h1>你好</h1>
<script>
$.fn.welcome = function(){
    return 'Hello World!!';
}

// $('h1')是$的一个实例，可以调用到welcome()方法
$('h1').html($('h1').welcome());
</script>
```

#### $.inArray(element, array, [fromIndex])
检测数组中有没有某个元素，有的话返回该元素的索引，没有的话返回-1。可选参数fromIndex代表从fromIndex这个索引开始查找。
```
var arr = ['red', 'black', 'blue', 'yellow'];
var bool1 = $.inArray('black', arr);
console.log(bool1); // 1
var bool2 = $.inArray('black', arr, 1);
console.log(bool2); // 1
var bool3 = $.inArray('black', arr, 2);
console.log(bool2); // -1
```

#### $.each(arr, function(index, item){ // 回调函数 })
遍历数组中的元素，回调函数返回false时停止遍历。
```
var arr = ['red', 'black', 'blue', 'yellow'];
$.each(arr, function(index, item){
    console.log('第%d个颜色是%s',index, item);
});
/*
第0个颜色是red
第1个颜色是black
第2个颜色是blue
第3个颜色是yellow
*/
```
    
#### $.map()
遍历数组，返回回调函数处理后的新数组。
```
var numArr = [1, 2, 3, 4, null, 5, undefined, 6];
var newNumArr = $.map(numArr, function(item, index){
    if(item){
        return item*item;
    }
});
console.log(newNumArr); // [1, 4, 9, 16, 25, 36]
```

#### 强行插入：$.each()和$.map()的区别
区别很简单：
+ `$.each()` 不会创建新数组，会在原始数组上进行操作。
+ `$.map()` 将会创建一个新数组，不操作原始数组。
+ 在没有必要的情况下使用 `$.map()` 可能会造成内存浪费。


#### $.parseJSON(string)
将标准的JSON数据<b>字符串</b>转换为JavaScript的对象。<i>注意：此处传入的参数是一个字符串。</i>

浏览器支持的转换方式：`JSON.parse(str);` 。效果相同。
```
// json字符串
var jsonStr = '{"name": "老王"}';

// 很容易就将json写成了这种格式，这会被JavaScript认为是一个对象，因此在执行$.parseJSON()时会报错。
var jsonObj = {"name": "老王"};

console.log($.parseJSON(jsonStr)); // 返回：Object {name: "老王"}
console.log($.parseJSON(jsonObj)); // Uncaught SyntaxError: Unexpected token o in JSON at position 1
```

JSON转字符串：`JSON.stringify(jsonObj)`。

#### $.trim()
删除字符串<b>首尾</b>的空白字符。
```
var str = ' Hello World ';
console.log(str.length); // 13
var trimStr = $.trim(str);
console.log(trimStr.length); // 11
```

#### offset()
两个功能：
+ 获取当前元素相对于document的位置。返回一个对象含有：top、right、bottom、left。
+ 设置当前元素相对于document的位置。

#### push()
添加元素到当前对象集合的后边。
```
var arr = [1, 2, 3];
arr.push(4);
console.log(arr);// [1, 2, 3, 4]
```

#### slice()
拆分数组，两种用法。从1开始计数。
+ slice(startIndex)返回到startIndex之后的元素组成的数组。
+ slice(startIndex, endIndex)返回startIndex到endIndex之间的元素组成的数组。
    ```
    var arr = [111, 222, 333, 444, 555];
    console.log(arr.slice(2)); //[333, 444, 555]
    console.log(arr.slice(2,5)); //[333, 444, 555]
    ```
## 核心API
包括AJAX、事件处理、FROM等比较重要的API及Zepto特有的API。

### 检测设备相关的API
注意：需要引入detect模块。
```
$.os.phone; // 检测是否为手机，成功返回true，失败返回false
$.os.tablet; // 检测是否为平板，成功返回true，失败返回false

// 检测系统
$.os.ios; // 检测是否为ios系统，成功返回true，失败返回undefined
$.os.android;  // 检测是否为android系统，成功返回true，失败返回undefined
$.os.webos;
$.os.blackberry;
$.os.bb10;
$.os.rimtabletos;

// 检测具体的设备
$.os.iphone; // 检测是否为iphone，成功返回true，失败返回undefined
$.os.ipad; // 检测是否为ipad，成功返回true，失败返回undefined
$.os.ipod; // 检测是否为ipod，成功返回true，失败返回undefined
$.os.touchpad;
$.os.kindle;

// 检测浏览器
$.browser.chrome; // 检测是否为chrome浏览器，成功返回true，失败返回undefined
$.browser.firefox;
$.browser.safari;
$.browser.webview;
$.browser.silk;
$.browser.playbook;
$.browser.ie;

// 检测版本
$.os.version;
$.browser.version;
```

### 事件处理相关的API
#### on
绑定事件。

#### off
移除事件。

#### one
绑定一次事件，执行一次后移除绑定。

难以理解的部分：
+ $.Event
+ $.proxy
+ trigger
+ triggerHandler
+ event.isDefaultPrevented
+ event.isImmediatePropagationStopped
+ event.isPropagationStopped

### AJAX相关的API
这部分没什么好说的，基本功。

### 表单相关的API

#### serialize
在Ajax post请求中将用作提交的表单元素的值编译成URL编码的字符串。

#### serializeArray
将用作提交的表单元素的值编译成拥有name和value对象组成的数组。不能使用的表单元素，buttons，未选中的radio buttons/checkboxs 将会被跳过。结果不包含file inputs的数据。

#### submit
为submit事件绑定一个处理函数。

### 效果相关的API
#### $.fx
全局的动画设置：
+ $.fx.off
+ $.fx.speeds：设置动画的时间
    + _defaults（400ms）
    + fast（200ms）
    + slow（600ms）
    
#### animate
动画的具体效果。    

### Touch
注意：需要引入touch模块。
```
<div class="demo" ></div>

<script>
$('.demo').longTap(function(){
    alert('hello touch');
})
</script>
```
可检测单击与双击。
+ tap点击触发，单击双击均可。
+ singleTap单击触发。
+ doubleTap双击触发。
+ longTap长按触发。
+ swipe滑动触发。

## End
一个一个Demo跑完了以后发现这是一篇索然无味的文章，Zepto的API基本和jQuery相同。

不过在移动端，Zepto显然更有优势，轻量是很重要的一点，为移动端量身定做的touch事件显然十分重要。

Zeopto官网的API是按照字母序排列的，自作聪明的将这些API归了下类，也许更适合新人拿来当闲书看吧。

## 番外篇
Build自己的zepto的方法。
1.从github下载完整安装包，[点击下载](https://github.com/madrobby/zepto/archive/master.zip)。
2.找到安装包里的make文件，修改下边这行代码，填入自己想要的模块。
```
modules = (env['MODULES'] || 'zepto event ajax form ie detect fx_methods data selector touch').split(' ')
```
3.运行`npm run-script dist` ，然后你就可以在dist文件夹下找到你的zepto版本。
 
