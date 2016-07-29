---
title: 开发规范
date: 2016-07-09 12:45:38
tags: [移动端]
---

工作中使用的移动端开发的规范，包括：HTML、SCSS、JavaScript等。
<!--more-->

<img src="http://tguide.qq.com/skins/foreground/assets/images/pic-home1.jpg" title="我也不知道为什么想放一张图片在这里">

## 如何编写军工级的代码？
个人认为应当具备如下条件：

+ 扎实的基本功。
+ 需要有一点代码强迫症，厌恶臃肿的代码，追求高质高效（然而家里堆了一大堆臭袜子(๑•̀ㅂ•́)و✧）。
+ 有一套成熟的规范，始终记着你不是一个人，团队中每个成员都应该恪守同一套开发规范。
+ 保持旺盛的积极性，不要让繁重的工作熄灭了你的热情！

> PostScript：作为一个职场 + 工作新人，自觉各方面均有较大程度的欠缺，本文权当作一个目标吧。
如有遗误，欢迎拍砖，期待与你交流。


## CodeReview
先来Review一下自己的代码吧？

### Part1
第一步：查看你的DOM数目，然后找个页面结构差不多的站（当然是技术比较成熟的站），比较一下。
```
document.getElementByTagName('*').length;
//xiguaaxigua首页是247，并不是我写的多好，而是页面简单，哈哈哈哈
```
相比之下，DOM数目越多说明你的页面里可能有一些冗余的标签。试着精简一下吧~

### Part2
第二步：查看一下你的布局结构，这个简单又实用，在之前的一篇文章中介绍过《[CSS奇淫技巧](http://xiguaaxigua.cn/2016/07/03/CSS-Skills/)》，去尝试一下吧~
```
*{ background-color: rgba(255,0,0,.2); }
**{ background-color: rgba(0,255,0,.2); }
***{ background-color: rgba(0,0,255,.2); }
****{ background-color: rgba(255,0,255,.2); }
*****{ background-color: rgba(0,255,255,.2); }
******{ background-color: rgba(255,255,0,.2); }
```

### Part3
第三步：查看页面里所有的元素，适合写完页面后的检查。
```
[].forEach.call($$("*"),function(a){a.style.outline="1px solid #"+(~~(Math.random()*(1<<24))).toString(16)})
```

这行代码简直短小精悍！

```
//先格式化一下

//获取一个随机的颜色值
var randColor = (~~(Math.random() * (1 << 24))).toString(16);

[].forEach.call(
		$$("*"), function(a) { 
			a.style.outline = "1px solid #" + randColor;
		}
	);
```
TIPS：
+ `[].forEach.call` 是为了获取到数组的forEach方法。
+ Chrome的控制台为了方便选择元素，可以通过$选择元素。
	+ $$("*")将会返回页面中所有的元素。
	+ $$("p")将会返回页面中所有的p元素。
	+ $$(".big-btn")将会返回页面中所有的class为big-btn元素。
+ call方法的用意是：让 `$$("*")` 的返回值（即页面所有的元素），去执行数组的forEach方法。
	+ 推荐文章《[JavaScript中的call和apply方法](http://blog.csdn.net/myhahaxiao/article/details/6952321)》。
+ outline不会影响布局。
+ 这段代码的用意是给页面所有的元素添加一个1px随机颜色的outline。

## 正文
本套开发规范借鉴了很多前辈的成果。For Example：
+ [NEC-更好的CSS方案](http://nec.netease.com/)
+ [编码规范](http://codeguide.bootcss.com/)
+ [移动Web开发规范](http://alloyteam.github.io/Spirit/modules/Standard/index.html)
+ [TGuide整体Web解决方案](http://tguide.qq.com/main/index.htm)
+ 以及同事兼好友@然哥的指导。

现将工作中（移动端）需要的部分记录如下。

### HTML部分
+ 1.所有的标签小写。
+ 2.使用 `html5` 的 `doctype` 。
+ 3.使用4个空格缩进代码。这是唯一能保证在所有环境下获得一致展现的方法。
+ 4.嵌套元素缩进一次。
+ 5.属性定义确保全部使用双引号，禁止使用单引号。
+ 6.禁止在自闭合元素的尾部添加斜线，例如：`<img src='' />`。
+ 7.禁止标签不闭合。
+ 8.注意HTML语义化、实用性、可扩展性，任何时候都要尽量少的使用标签，并保持最小的复杂度。
+ 9.属性的顺序，应当保持高可阅读性，例如：`id > class > data-* > src/for/type/href > title/alt`。
+ 10.为HTML根元素指定 `lang` 属性（多语言忽略此条）。
+ 11.引入CSS和JavaScript不需要指定 `type` 属性。
+ 12.布尔属性无需赋值，例如：`checked` ， `disabled` 。
+ 13.元素的嵌套不能随意为之。
    + 段落元素与标题元素只能嵌套内联元素。
    + 能并列书写就不要嵌套。
+ 14.尽量避免使用JavaScript生成标签，降低性能的同事也会增大维护的难度。
+ 15.文件的命名以该页面的功能为基准。
+ 16.引用文件时，后缀名必须小写。
+ 17.合理使用注释，禁止在注释中出现 `--` ，注释位于代码块上方，两头有空格。
    ```
    <!-- 正确的注释姿势 -->
    <h1>Hello Wrold</h1>
    ```
+ 18.对 `<` ， `>` 之类的符号进行转义。
+ 19.每一个块元素都另起一行。
+ 20.保持良好的树形结构，可以使用编辑器格式化后再PUSH。

### CSS规范
工作中使用的SASS。

+ 1.使用4个空格代替制表符。
+ 2.正确的书写姿势，注意空格的使用。
    ```
         body {
            font-size: 12px;
            li {
                line-height: 1.5;
                color: #fff;
            }
        }
    ```
+ 3.十六进制的颜色应该全部小写，能简写的尽量简写，如 `#FFFFFF` 应当写为： `#fff` 。
+ 4.禁止为0值添加单位，如：`margin-top: 0px;` 应该写为： `margin-top: 0;`。
+ 5.命名、属性、值全部小写，连字符使用 `-`。
+ 6.命名应简约而不失语义，所有的class和id后必须添加工号。
    ```
    /* 正确的姿势 */
    header-box751 {
        position: fixed;
        .header-top {
            line-height: 2;      
        }
    }
    ```
+ 7.表现和行为分开，class控制样式，需要JS控制的标签添加一个新的ID类似于这样： `J_toggleBanner751` 。
    + 必须 `J_` 开头。
    + 必须保证该页面id的唯一性。
    + 必须有工号。
    + 必须使用小驼峰。
+ 8.属性应符合一定的顺序，不能随意为之，此处借助于编辑器，比如Sublime中的 `csscomb` 插件。
    ```
    /* 正确的顺序 */
    定位 > 盒模型 > 印刷 > 视觉
    ```
    TIPS：定位可以从正常的文档流中移除元素，并且能覆盖盒模型的相关样式。盒模型排在第二位，因为它决定了组件的尺寸和位置。
+ 9.编写良好的注释。
+ 10.SASS中减少嵌套的层次。
+ 11.样式文件的书写应参照一定的规则。
	+ 重置（reset）和默认（base）。
	+ 统一处理：同一调用背景图、清除浮动等。
	+ 布局（grid）（`.g-`）。
	+ 模块（module）（.m-）：通常是一个语义化的可以重复使用的较大的整体！比如导航、登录、注册、各种列表、评论、搜索等！
	+ 元件（unit）（.u-）：通常是一个不可再分的较为小巧的个体，通常被重复用于各种模块中！比如按钮、输入框、loading、图标等！
	+ 功能（function）（.f-）。
	+ 皮肤（skin）（.s-）。
	+ 状态（.z-）。
+ 12.省略url中的引号，需要使用引号的地方必须使用单引号，比如： `font-family: '黑体';` 。
+ 13.避免耗费性能的属性。比如：
	```
		.class{width:expression(this.width>100?'100px':'auto');}
	```
+ 14.能用CSS做的就不要用JS。
+ 15.同一语义理解和命名，前辈们已经总结的很好，[点击查看](http://nec.netease.com/standard/css-practice.html)。
+ 16.不要使用 `@import` 。

### JavaScript规范
+ 1.移动端使用Zepto，除此库之外，应尽量减少第三方库的引入。
+ 2.尽量使用原生JavaScript，提高代码性能。
+ 3.避免污染全局环境，减少在全局环境定义变量、函数。
+ 4.所有变量的命名使用小驼峰，原则上采用动词及动词短语，以描述该函数的行为。
+ 5.遵循ES6标准，后期会转换为ES5。
+ 6.常量使用下划线分割的大写字符串来定义，例如： `COOKIE_PREFIX`。
+ 7.类及命名空间使用大驼峰。
+ 8.所有的语句必须用分号结尾所有的代码块不得使用分号结尾。
+ 9.合理使用注释。
+ 10.保持代码的精简。
+ 11.个人习惯总是将代码包裹成一个IIFE。

### 需要适配的浏览器：
+ 安卓原生浏览器
+ Safari
+ Chrome
+ Opera Mini
+ UC Brower
+ QQ Brower

## End
“说时容易做时难”，以后定当恪守规范，严格执行。

另外，对原生JavaScript这块欠缺甚多，注意注意。