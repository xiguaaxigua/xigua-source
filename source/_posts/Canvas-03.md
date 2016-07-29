---
title: 第三个Canvas—时钟
date: 2016-07-14 20:26:58
tags: [Canvas]
---

Canvas进阶之路。
<!--more-->

效果图：
![](http://7xtoaz.com1.z0.glb.clouddn.com/canvas3.jpg)

Canvas-倒计时Demo页面：[点击访问](http://7xtoaz.com1.z0.glb.clouddn.com/Canvas-03.html)

## 正文

### 代码实现
很简单，一个html一个脚本。脚本中的注释还算详细吧。
HTML部分：
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Canvas-时钟</title>
</head>
<body>
	<canvas id="canvas" width="500" height="400"></canvas>

	<script src="demo.js"></script>
</body>
</html>
```
JavaScript部分：
```
//console.log('Hello Canvas!');

var Canvas = {};
Canvas.context = document.getElementById('canvas').getContext('2d');

Canvas.Point = function(x, y){
	this.x = x;
	this.y = y;
};
// 清空画布
Canvas.clearCxt = function(){
	var me = this;
	var canvas = me.context.canvas;
	me.context.clearRect(0, 0, canvas.offsetWidth, canvas.offsetHeight);
}

// 时钟
Canvas.Clock = function(){
	var me = Canvas,
		c = me.context,
		// 半径
		radius = 150,
		// 刻度 
		scale = 20,
		// 每分钟的弧度
		minAngle = (1 / 30) * Math.PI,
		// 每小时的弧度
		hourAngle = (1 / 6) * Math.PI,
		// 时针长度
		hourLength = radius / 2,
		// 分针长度
		minLength = radius / 3 * 2,
		// 秒针长度
		secLength = radius / 10 * 9,
		// 圆心坐标
		center = new me.Point(c.canvas.width / 2, c.canvas.height / 2);

	// 绘制表盘中心
	function drawCenter(){
		c.save();

		c.translate(center.x, center.y);

		c.fillStyle = '#f3a829';
		c.strokeStyle = '#f3a829';

		c.beginPath();
		c.arc(0, 0, radius / 15, 0, 2 * Math.PI);
		c.closePath();

		c.fill();
		c.stroke();
		c.restore();
	}

	// 绘制表盘
	function drawBackground(){
		c.save();

		c.translate(center.x, center.y);

		// 绘制刻度
		function drawScale(num, width){

			c.fillStyle = '#000';
			c.lineWidth = 3;
			c.moveTo(radius - num*scale, 0);
            c.lineTo(radius, 0); 
		};

		c.beginPath();
		//c.arc(0, 0, radius, 0, 2 * Math.PI, true);
		c.closePath();

		for(var i=1; i<=60; i++){

			if( i%5 == 1){
				c.strokeStyle = 'rgba(29, 149, 49, 0.8)';
				drawScale(1.1);
			}else{
				drawScale(0.4);
			}

			// 旋转画布
			c.rotate(hourAngle / 5);
		};

		// 绘制时间
		c.font = "18px Verdana";
		c.fillText("1", 56, -95);
		c.fillText("2", 96, -55);
		c.fillText("3", 115, 6);
		c.fillText("4", 96, 66);
		c.fillText("5", 56, 110);
		c.fillText("6", -4, 120);
		c.fillText("7", -56, 110);
		c.fillText("8", -106, 70);
		c.fillText("9", -125, 6);
		c.fillText("10", -106, -55);
		c.fillText("11", -66, -95);
		c.fillText("12", -10, -110);
		c.stroke();

		c.restore();
	}

	// 绘制时针
	this.drawHourHand = function(h){

		h = h === 0 ? 24 : h;

		c.save();
		c.translate(center.x, center.y);
		c.rotate(3/2*Math.PI);

		c.rotate(h*hourAngle);


		c.lineWidth = '8';
		c.fillStyle = '#222222';
		c.beginPath();
		c.moveTo(0, 0);
		c.lineTo(hourLength, 0);
		c.closePath();

		c.stroke();
		c.restore();
	}

	// 绘制分针
	this.drawMinHand = function(m){

		m = m === 0 ? 60 : m;

		c.save();
		c.translate(center.x, center.y);
		c.rotate(3/2*Math.PI);
		c.rotate(m*minAngle);

		c.lineWidth = '5';

		c.beginPath();
		c.moveTo(0, 0);
		c.lineTo(minLength, 0);
		c.closePath();

		c.stroke();
		c.restore();
	}

	// 绘制秒针
	this.drawSecHand = function(s){

		s = s === 0 ? 60 : s;

		c.save();
		c.translate(center.x, center.y);
		c.rotate(3/2*Math.PI);
		c.rotate(s*minAngle);

		c.lineWidth = '3';
		c.strokeStyle = '#f3a829';

		c.beginPath();
		c.moveTo(-20, 0);
		c.lineTo(secLength, 0);
		c.closePath();

		c.stroke();
		c.restore();
	}

	this.drawText = function(){
		c.beginPath();
		c.font="16px Verdana";
		c.fillStyle = '#1d9531';
		c.fillText("Hello Canvas!", 200, 250);

		c.closePath();
		c.fill();

		c.stroke();
		c.restore();
	}

	// 
	this.drawClock = function(){
		var me = this;
		function draw(){
			var date = new Date();

			Canvas.clearCxt();
			drawBackground();
			me.drawText();
			me.drawHourHand(date.getHours() + date.getMinutes()/60);
			me.drawMinHand(date.getMinutes() + date.getSeconds()/60);
			me.drawSecHand(date.getSeconds());
			drawCenter();
		}
		draw();
		setInterval(draw, 1000);
	}

}

var main = function(){
	var clock = new Canvas.Clock();
	clock.drawClock();
}

window.onload = function(){
	main();
}
```

### 要点
+ 刻度绘制，通过旋转画布实现。
+ 每小时、每分钟的弧度的计算。
    ```
    // 每分钟的弧度
    minAngle = (1 / 30) * Math.PI,
    // 每小时的弧度
    hourAngle = (1 / 6) * Math.PI,
    ```
+ 注意canvas绘制先后顺序，决定了两个图片哪个位于上方。
+ 表盘上时间的绘制，将中心平移到表盘的中心后计算。
+ 填充颜色：`c.fillStyle = '#f00'`。 边框颜色：`c.strokeStyle='#000'`。
+ 注意怎么区分当前刻度是否为3、6、9、12点。
+ JavaScript的作用域还是迷迷糊糊的。

## End
That`s aLL.