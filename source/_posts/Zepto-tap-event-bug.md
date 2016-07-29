---
title: Zepto中tap事件的“穿透”问题
date: 2016-07-17 16:12:24
tags: [框架]
---

什么是“穿透”bug；怎么解决它。
<!-- more -->

## “穿透”是什么
代码：
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<style>
	.inner{
	    width: 100px;
	    height: 100px;
	    background: #f00;
	    position: absolute;
	    left: 0;
	    top: 0;
	}
	.outter{
	    width: 200px;
	    height: 200px;
	    background: green;
	    position: absolute;
	    left: 0;
	    top: 0;
	}
	</style>
</head>
<body>
	<div class="outter">
        
    </div>
    <div class="inner">
        
    </div>
	<script src="../zepto.min.js"></script>
	<script>
        
        $('.inner').tap(function(){
            $(this).hide();
        });
        $('.outter').click(function(){
            alert('你点到我了');
        });
	</script>
</body>
</html>
```

可以看到，当点击inner的时候，inner会消失，但是也会触发绑定在outter上的alert事件。这就是Zepto中tap事件的“穿透”bug。

### 解决办法1
用touchend事件代替tap事件，并阻止touchend默认的行为。
```
$('.inner').on('touchend', function(e){
    $(this).hide();

    e.preventDefault();
});
```

### 解决办法2
延迟处理tap后需要处理的事件。这种方式适合添加一些渐变，但大多数情况下并没有解决“300ms延迟”的问题。
```
$('.inner').on('tap', function(e){
            
    setTimeout(function(){
        $('.inner').hide();
    }, 320);
});
```

### 解决办法3
使用FastClick插件，项目地址：[https://github.com/ftlabs/fastclick](https://github.com/ftlabs/fastclick)。

原生js：
```
window.addEventListener('load', function() {
  FastClick.attach(document.body);
}, false);
```

Zepto中：
```
$(function() {
  FastClick.attach(document.body);
});
```

RequireJS中：
```
var attachFastClick = require('fastclick');
attachFastClick(document.body);
```

## End
既然Zepto存在bug为什么这么久了都没有人修复呢？为什么不借鉴FastClick的思想呢？
