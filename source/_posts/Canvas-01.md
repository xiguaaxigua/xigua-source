---
title: 第一个Canvas—七巧板
date: 2016-07-12 20:40:16
tags: [Canvas, 小情绪]
---

Canvas进阶之路。
<!--more-->

## 我为什么突然捣鼓Canvas去了

此处应该放一首歌：
<audio src="http://7xtoaz.com1.z0.glb.clouddn.com/%E8%BF%BD%E6%A2%A6%E8%B5%A4%E5%AD%90%E5%BF%83.mp3" controls></audio>

近日，忧心忡忡不可终日。

同万千刚参加工作的新人一样，想要去接触的知识繁多而杂乱、不成章法，从Angular、Backbone、Node到各种UI框架，流行的东西总想着参和一腿子，工作忙没时间去捣鼓还会像个小媳妇儿一样内疚，偶尔还会冒出个想法学学写JavaScript插件... 

长期浪迹于知乎、各种技术交流群，AND各种国人外国人的技术博客，微信也关注了一堆公众号，让人有些眼花缭乱
。另一方面，个人追崇极简主义，强迫症中期，以至于凡事讲究大道至简，做事妄想一蹴而就，盲目的追求高质量高逼格（事实证明并没有高到哪里去）。种种的原因，让人难以安眠。

要知道：
+ 千里之行始于足下，望山跑死马。
+ 你的努力程度之低，远远不到拼天赋的时候。
+ 只有梦想能支撑你走完这一生！

SO...与其瞎逼操心，不如做个勤学好问的小学生。那么就从canvas开始吧。

## 一个小规划
+ Canvas进阶之路，暂定一个月，即8月12号中止，开始下一阶段。
+ 过一遍JavaScript，来几套面试题目压压惊。9月12号中止。
    重点在底层基础概念，次重点ES6。
+ Node进阶之路，一个月。10月12号中止。
+ 读书《JavaScript动画设计》，半个月。11月底中止。
+ 重拾Angular，一个半月，大致在。1月中旬中止。
+ 了解React、Backbone、VueJS，时间暂定。
+ Maybe，中途会异想天开捣鼓点别的东西，大致规划如此。

## Canvas-七巧板
效果图：
![](http://7xtoaz.com1.z0.glb.clouddn.com/canvas1.jpg)

### Canvas简介
html5的新标签，用于图形的绘制，结合JavaScript可以达到各种炫酷的效果。
收集到的一些canvas效果站：
+ [canvas特效代码](http://www.17sucai.com/pins/tag/3886.html)
+ 后续看到会陆续补充。

### 七巧板代码
```
<canvas id="canvas" width="600" height="600" style="margin: 0 auto; border: 1px solid #000;"></canvas>
<script>

var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d'); 

// blue
ctx.beginPath();
ctx.moveTo(0, 0);
ctx.lineTo(300, 300);
ctx.lineTo(0, 600);
ctx.lineTo(0, 0);
ctx.closePath();

ctx.fillStyle = 'blue';
ctx.fill();

// yellow
ctx.beginPath();
ctx.moveTo(0, 0);
ctx.lineTo(300, 0);
ctx.lineTo(150, 150);
ctx.lineTo(0, 0);
ctx.closePath();

ctx.fillStyle = "yellow";
ctx.fill();

// green
ctx.beginPath();
ctx.moveTo(300, 0);
ctx.lineTo(600, 0);
ctx.lineTo(600, 300);
ctx.lineTo(300, 0);
ctx.closePath();

ctx.fillStyle = "green";
ctx.fill();

// pink
ctx.beginPath();
ctx.moveTo(0, 600);
ctx.lineTo(600, 600);
ctx.lineTo(300, 300);
ctx.lineTo(0, 600);

ctx.closePath();
ctx.fillStyle = "pink";
ctx.fill();

// red
ctx.beginPath();
ctx.moveTo(300, 300);
ctx.lineTo(450, 150);
ctx.lineTo(450, 450);
ctx.lineTo(300, 300);
ctx.closePath();
ctx.fillStyle = "red";
ctx.fill();

// orange
ctx.beginPath();
ctx.moveTo(300, 0);
ctx.lineTo(450, 150);
ctx.lineTo(300, 300);
ctx.lineTo(150, 150);
ctx.lineTo(300, 0);
ctx.closePath();

ctx.fillStyle = "orange";
ctx.fill();

// black
ctx.beginPath();
ctx.moveTo(600, 600);
ctx.lineTo(600, 300);
ctx.lineTo(450, 150);
ctx.lineTo(450, 450);
ctx.closePath();
ctx.fillStyle = "black";
ctx.fill();
</script>
```

## End
年轻人应当有点年轻人的样子，加油吧！