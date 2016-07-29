---
title: 第二个Canvas—倒计时
date: 2016-07-13 20:05:45
tags: [Canvas]
---

Canvas进阶之路。
<!--more-->

学习的第一步是模仿，先来个效果图：
![](http://7xtoaz.com1.z0.glb.clouddn.com/canvas2.jpg)

Canvas-倒计时Demo页面：[点击访问](http://7xtoaz.com1.z0.glb.clouddn.com/Canvas2.html)

## 正文

### 整体逻辑

+ 1.事先设定好一个时间，推算出需要倒计时的时间。博主的Demo里写死的3个小时。
+ 2.根据点阵数组（姑且这么叫吧，看到[源代码](http://7xtoaz.com1.z0.glb.clouddn.com/canvas2-Digit.js)你就知道怎么回事了）绘制每一个时间元素及冒号，比如：`12:01:00` 中，每一个数字和冒号都要依序绘制。
+ 3.检测时间的变化，并重新绘制canvas，实现倒计时的效果。
+ 4.检测时间的变化，在变化的数字处生成随机颜色、随机速度、随机加速度的小球。

### 代码实现
Demo分为三个文件：
+ demo.html
+ digit.js
+ demo.js

demo.html部分：
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Canvas-倒计时</title>
</head>
<body>
	<canvas id="canvas" style="margin: 0 auto"></canvas>

	<script src="digit.js"></script>
	<script src="demo.js"></script>
</body>
</html>
```

digit.js部分不再赘述，自己查看源代码。

demo.js部分：
```
!(function() {
    var canvasWidth = 900;
    var canvasHeight = 300;
    var canvasMarginTop = 20;
    var canvasMarginLeft = 120;
    var nowTime = new Date();
    var endTime = new Date(nowTime.getTime() + 3 * 60 * 60 * 1000);
    var digitColor = 'rgba(29, 149, 49, 0.5)';
    var digitRadius = 5;
    var balls = [];
    var colors = ["#980000", "#ff0000", "#ffff00", "#00ff00", "#00ffff", "#4a86e8", "#0000ff", "#9900ff", "#ff00ff"];
    var textLeft = 120;
    var textTop = 200;
    var text = '— Hello Canvas!';

    var canvas = document.getElementById('canvas');
    var context = canvas.getContext('2d');

    canvas.width = canvasWidth;
    canvas.height = canvasHeight;
    canvas.style.display = 'block';
    canvas.style.margin = '0 auto';

    var showTime = getCurrentTime();

    setInterval(
        function() {
            render(showTime, context);
            update(context);
        }, 50);

    // 返回倒计时的时间
    function getCurrentTime() {
        var currentTime = new Date();

        // getTime()获得时间戳
        var res = endTime.getTime() - currentTime.getTime();

        // 获得秒数
        res = Math.round(res / 1000);

        return res >= 0 ? res : 0;
    }

    // 渲染当前时间
    function render(showTime, context) {
        context.clearRect(0, 0, canvasWidth, canvasHeight);

        var hours = parseInt(showTime / 3600);
        var minutes = parseInt((showTime - hours * 3600) / 60);
        var seconds = showTime % 60;

        // 小时
        renderDigit(canvasMarginLeft, canvasMarginTop, parseInt(hours / 10), context);
        renderDigit(canvasMarginLeft + 15 * (digitRadius + 1), canvasMarginTop, parseInt(hours % 10), context);

        // 冒号
        renderDigit(canvasMarginLeft + 30 * (digitRadius + 1), canvasMarginTop, 10, context);

        // 分钟
        renderDigit(canvasMarginLeft + 39 * (digitRadius + 1), canvasMarginTop, parseInt(minutes / 10), context);
        renderDigit(canvasMarginLeft + 54 * (digitRadius + 1), canvasMarginTop, parseInt(minutes % 10), context);

        //冒号
        renderDigit(canvasMarginLeft + 69 * (digitRadius + 1), canvasMarginTop, 10, context);
        renderDigit(canvasMarginLeft + 78 * (digitRadius + 1), canvasMarginTop, parseInt(seconds / 10), context);
        renderDigit(canvasMarginLeft + 93 * (digitRadius + 1), canvasMarginTop, parseInt(seconds % 10), context);

        for (var i = 0; i < balls.length; i++) {
            context.fillStyle = balls[i].color;

            context.beginPath();
            context.arc(balls[i].x, balls[i].y, digitRadius, 0, 2 * Math.PI, true);
            context.closePath();

            context.fill();
        }
    }

    // 渲染球
    function renderDigit(x, y, num, context) {

        context.fillStyle = digitColor;

        for (var i = 0; i < digit[num].length; i++) {
            for (var j = 0; j < digit[num][i].length; j++) {

                if (digit[num][i][j] == 1) {
                    context.beginPath();
                    context.arc(x + j * 2 * (digitRadius + 1) + (digitRadius + 1), y + i * 2 * (digitRadius + 1) + (digitRadius + 1), digitRadius, 0, 2 * Math.PI)
                    context.closePath();

                    context.fill();
                }
            }
        }
    }

    function update(context) {
        var nextShowTime = getCurrentTime();

        var nextHours = parseInt(nextShowTime / 3600);
        var nextMinutes = parseInt((nextShowTime - nextHours * 3600) / 60);
        var nextSeconds = nextShowTime % 60;

        var hours = parseInt(showTime / 3600);
        var minutes = parseInt((showTime - hours * 3600) / 60);
        var seconds = showTime % 60;

        if (nextSeconds != seconds) {
            if (parseInt(hours / 10) != parseInt(nextHours / 10)) {
                addBalls(canvasMarginLeft, canvasMarginTop, parseInt(hours / 10));
            }
            if (parseInt(hours % 10) != parseInt(nextHours % 10)) {
                addBalls(canvasMarginLeft + 15 * (digitRadius + 1), canvasMarginTop, parseInt(hours / 10));
            }
            if (parseInt(minutes / 10) != parseInt(nextMinutes / 10)) {
                addBalls(canvasMarginLeft + 39 * (digitRadius + 1), canvasMarginTop, parseInt(minutes / 10));
            }
            if (parseInt(minutes % 10) != parseInt(nextMinutes % 10)) {
                addBalls(canvasMarginLeft + 54 * (digitRadius + 1), canvasMarginTop, parseInt(minutes % 10));
            }
            if (parseInt(seconds / 10) != parseInt(nextSeconds / 10)) {
                addBalls(canvasMarginLeft + 54 * (digitRadius + 1), canvasMarginTop, parseInt(seconds % 10));
            }
            if (parseInt(seconds % 10) != parseInt(nextSeconds % 10)) {
                addBalls(canvasMarginLeft + 93 * (digitRadius + 1), canvasMarginTop, parseInt(seconds % 10));
            }
            showTime = nextShowTime;
        }
        updateBalls();
        renderText(context);
    }

    function updateBalls() {
        for (var i = 0; i < balls.length; i++) {
            balls[i].x += balls[i].vx;
            balls[i].y += balls[i].vy;
            balls[i].vy += balls[i].g;

            // 碰撞检测
            if (balls[i].y >= canvasHeight - digitRadius) {
                balls[i].y = canvasHeight - digitRadius;
                balls[i].vy = -balls[i].vy * 0.6;
            }
        }
    }

    function addBalls(x, y, num) {
        for (var i = 0; i < digit[num].length; i++) {
            for (var j = 0; j < digit[num][i].length; j++) {

                if (digit[num][i][j] == 1) {
                    var aBall = {
                        x: x + j * 2 * (digitRadius + 1) + (digitRadius + 1),
                        y: y + i * 2 * (digitRadius + 1) + (digitRadius + 1),
                        g: 1.5 + Math.random(),
                        vx: Math.pow(-1, Math.ceil(Math.random() * 1000)) * 5,
                        vy: -5,
                        color: colors[Math.floor(Math.random() * colors.length)]
                    }
                    balls.push(aBall);
                }
            }
        }
    }

    function renderText(context) {
        // 文字
        context.font = '30px 微软雅黑';
        context.fillStyle = '#1d9531';
        context.fillText(text, textLeft, textTop);
    }

})();
```

### 要点
+ `var endTime = new Date(2016, 6, 14, 00, 00, 00)` 当前创建的时间中的月份是7月而不是6月！0是一月，好坑啊。
+ 随机获取 `5` 或者 `-5`：
    ```
    var randNum= Math.pow(-1, Math.ceil(Math.random() * 1000)) * 5;
    ```
+ 清空指定canvas中指定位置的图像：[W3C中的解释](http://www.w3school.com.cn/tags/canvas_clearrect.asp)
    ```
    var canvasWidth = 1000;
    var canvasHeight = 500;
    var canvas = document.getElementById('canvas');
    var context = canvas.getContext('2d');
    canvas.width = canvasWidth;
    canvas.height = canvasHeight;
    context.clearRect(0, 0, canvasWidth, canvasHeight);
    ```
+ 循环：`setInterval(function(){}, 100);` 。
+ 圆的绘制：[W3C中的解释](http://www.w3school.com.cn/tags/canvas_arc.asp)
    ```
    context.arc(圆心x坐标, 圆心y坐标, 半径, 起始角, 结束角, 绘制方向);
    ```
+ 文字的绘制：[W3C中的解释](http://www.w3school.com.cn/tags/canvas_filltext.asp)
    ```
    context.fillText(要绘制的文本, 开始绘制处的x坐标, 开始绘制处的y坐标, 允许的最大文本宽度);
    ```
+ 获取时间，通过时间戳获取秒数，并计算小时、分钟、秒数。

## End
Canvas是JavaScript和高数的碰撞！