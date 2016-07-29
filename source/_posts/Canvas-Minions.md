---
title: 第四个Canvas—小黄人
date: 2016-07-20 21:11:22
tags: [Canvas]
---

画个小黄人哄女友。嘿嘿嘿。
<!-- more -->

效果图：
![](http://7xtoaz.com1.z0.glb.clouddn.com/canvas4.jpg)

Canvas-小黄人页面，[点击访问](http://7xtoaz.com1.z0.glb.clouddn.com/Canvas-Minions.html)。

## 正文
实现思路，小黄人是像素画，可以看到由黄色、黑色、蓝色的正方形组成。因此，只要在合适的位置画一个正方形就可以了。

先定义几个颜色：
```
var white = '#fff',
	yellow = '#FCFA3A', 
	blue = '#4FAAFB', 
	black = '#0F0E0F',
	green = '#1d9531';
```

那么怎么确定正方形的位置呢？我把这些位置定义在了一个二维数组中。arr[i]代表小黄人的每一行，arr[i][j]代表每一行里的每一个方块。0表示填充颜色为白色，1表示填充颜色为黄色，2表示填充颜色为黑色，3表示填充颜色为蓝色。
```
var arr = [
	[0, 0, 1, 1, 1, 1, 1, 1, 0, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 1, 1, 2, 1, 1, 2, 1, 1, 0],
	[0, 2, 2, 0, 2, 2, 0, 2, 2, 0],
	[0, 1, 1, 2, 1, 1, 2, 1, 1, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[1, 1, 1, 1, 2, 2, 1, 1, 1, 1],
	[1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
	[1, 3, 3, 3, 3, 3, 3, 3, 3, 1],
	[1, 3, 3, 3, 3, 3, 3, 3, 3, 1],
	[2, 3, 3, 3, 3, 3, 3, 3, 3, 2],
	[0, 3, 3, 3, 3, 3, 3, 3, 3, 0],
	[0, 0, 3, 3, 3, 3, 3, 3, 0, 0],
	[0, 0, 0, 3, 0, 0, 3, 0, 0, 0],
	[0, 0, 2, 2, 0, 0, 2, 2, 0, 0]
];
```

再写一个画正方形的方法：x为起点横坐标，y为起点纵坐标，color为当前正方形的填充颜色，context为上下文环境。
```
function drawSquare(x, y, color, context){
	context.beginPath();
	context.fillStyle = color;
	context.fillRect(x+2, y+2, 16, 16);
	context.closePath();
}
```

万事俱备，现在开始循环数组：
```
for(var i=0; i< arr.length; i++){
	var x = 0;
	var y = 0;
	var color = '';
	y = 20 * i;

	for(var j=0; j< arr[i].length; j++){
		x = 20 * j;
		switch (arr[i][j]){
			case 0:
				color = white;
				break;
			case 1:
				color = yellow;
				break;
			case 2: 
				color = black;
				break;
			case 3:
				color = blue;
				break;
		}
		drawSquare(x, y, color, context);
	}
}
```

怎么样？是不是好简单？快去画一个哄孩子哄女友吧（然而并不好使）。

完整代码：

HTML:
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Canvas-小黄人</title>
</head>
<body>
	<canvas id="canvas" width="200" height="370"></canvas>

	<script src="demo.js"></script>
</body>
</html>
```

js:
```

var canvas = document.getElementById('canvas');
canvas.style.display = 'block';
canvas.style.margin = '0 auto';
var context = canvas.getContext('2d');

var white = '#fff',
	yellow = '#FCFA3A', 
	blue = '#4FAAFB', 
	black = '#0F0E0F',
	green = '#1d9531';

function drawSquare(x, y, color, context){
	context.beginPath();
	context.fillStyle = color;
	context.fillRect(x+2, y+2, 16, 16);
	context.closePath();
}

function drawText(green, context){
	context.font = '18px 微软雅黑';
	context.fillStyle = green;
	context.fillText('Hello Minions!', 37, 350);
}

var arr = [
	[0, 0, 1, 1, 1, 1, 1, 1, 0, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[0, 1, 1, 2, 1, 1, 2, 1, 1, 0],
	[0, 2, 2, 0, 2, 2, 0, 2, 2, 0],
	[0, 1, 1, 2, 1, 1, 2, 1, 1, 0],
	[0, 1, 1, 1, 1, 1, 1, 1, 1, 0],
	[1, 1, 1, 1, 2, 2, 1, 1, 1, 1],
	[1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
	[1, 3, 3, 3, 3, 3, 3, 3, 3, 1],
	[1, 3, 3, 3, 3, 3, 3, 3, 3, 1],
	[2, 3, 3, 3, 3, 3, 3, 3, 3, 2],
	[0, 3, 3, 3, 3, 3, 3, 3, 3, 0],
	[0, 0, 3, 3, 3, 3, 3, 3, 0, 0],
	[0, 0, 0, 3, 0, 0, 3, 0, 0, 0],
	[0, 0, 2, 2, 0, 0, 2, 2, 0, 0]
];

for(var i=0; i< arr.length; i++){
	var x = 0;
	var y = 0;
	var color = '';
	y = 20 * i;

	for(var j=0; j< arr[i].length; j++){
		x = 20 * j;
		switch (arr[i][j]){
			case 0:
				color = white;
				break;
			case 1:
				color = yellow;
				break;
			case 2: 
				color = black;
				break;
			case 3:
				color = blue;
				break;
		}
		drawSquare(x, y, color, context);
	}

}
drawText(green, context);
```