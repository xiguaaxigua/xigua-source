---
title: 常用原生JS方法总结（兼容性写法）
date: 2016-05-05 11:51:26
tags: [JavaScript, 文摘]
---
记录一些JS方法。
<!--more-->

### 添加事件方法
``` javascript
addHandler：function(element,type,handler){

	if(element.addEventListener){//检测是否为DOM2级方法

		element.addEventListener(type, handler,false);

	}elseif(element.attachEvent){//检测是否为IE级方法

		element.attachEvent("on"+ type, handler);

	}else{//检测是否为DOM0级方法

		element["on"+ type] = handler;

	}

}
```

### 移除之前添加的事件方法
``` javascript
removeHandler：function(element, type, handler){

	if(element.removeEventListener){

		element.removeEventListener(type, handler,false);

	}elseif(element.detachEvent){

		element.detachEvent("on"+ type, handler);

	}else{

		element["on"+ type] =null;

	}

}
```

### 获取事件及事件对象目标
``` javascript
//获取事件对象的兼容性写法

getEvent:function(event){

	returnevent ? event : window.event;

},

//获取事件对象目标的兼容性写法

getTarget:function(event){

	returnevent.target || event.srcElement;

}
```

### 阻止浏览器默认事件的兼容性写法
``` javascript
preventDefault:function(event){

	if(event.preventDefault){

		event.preventDefault();

	}else{

		event.returnValue =false;

	}

}
```

### 阻止事件冒泡的兼容性写法
``` javascript
stopPropagation:function(event){

	if(event.stopPropagation){

		event.stopPropagation();

	}else{

		event.cancelBubble =true;

	}

}
```

### mouseover和mouseout 事件才包含的获取相关元素的方法
``` javascript
//mouseover和mouseout 事件才包含的获取相关元素的方法

getRelatedTarget:function(event){

	if(event.relatedTarget){

		returnevent.relatedTarget;

	}elseif(event.toElement){

		returnevent.toElement;

	}elseif(event.fromElement){

		returnevent.fromElement;

	}else{

		returnnull;

	}

}
```

### 鼠标滚轮判断
``` javascript
/*对于mousedown 和mouseup 事件来说，则在其event 对象存在一个button 属性，

表示按下或释放的按钮。DOM的button 属性可能有如下3 个值：0 表示主鼠标按钮，1 表示中间的鼠

标按钮（鼠标滚轮按钮），2 表示次鼠标按钮。在常规的设置中，主鼠标按钮就是鼠标左键，而次鼠标

按钮就是鼠标右键。

IE8 及之前版本也提供了button 属性，但这个属性的值与DOM 的button 属性有很大差异。

 0：表示没有按下按钮。

 1：表示按下了主鼠标按钮。

 2：表示按下了次鼠标按钮。

 3：表示同时按下了主、次鼠标按钮。

 4：表示按下了中间的鼠标按钮。

 5：表示同时按下了主鼠标按钮和中间的鼠标按钮。

 6：表示同时按下了次鼠标按钮和中间的鼠标按钮。

 7：表示同时按下了三个鼠标按钮。*/

getButton:function(event){

	if(document.implementation.hasFeature("MouseEvents","2.0")){

		returnevent.button;

	}else{

		switch(event.button){

			case0:

			case1:

			case3:

			case5:

			case7:

			return0;

			case2:

			case6:

			return2;

			case4:

			return1;

			}

	}

}
```

### 能够取得鼠标滚轮增量值（delta）的方法
``` javascript
getWheelDelta:function(event){

	if(event.wheelDelta){

		return(client.engine.opera && client.engine.opera < 9.5 ?

		-event.wheelDelta : event.wheelDelta);

	}else{

		return-event.detail * 40;//firefox中的值为+3表示向上滚，-3表示向下滚

	}

}
```

### 跨浏览器的方式取得字符编码
``` javascript
getCharCode:function(event){

	if(typeofevent.charCode =="number"){

		returnevent.charCode;

	}else{

		returnevent.keyCode;

	}

}
```

### 访问剪贴板中的数据
``` javascript
getClipboardText:function(event){

	varclipboardData = (event.clipboardData || window.clipboardData);

	returnclipboardData.getData("text");

}
```

### 设置剪贴板中的数据
``` javascript
setClipboardText:function(event, value){

	if(event.clipboardData){

		returnevent.clipboardData.setData("text/plain", value);

	}elseif(window.clipboardData){

		returnwindow.clipboardData.setData("text", value);

	}

}
```



<p style="text-align: right;">[原文地址](http://www.kuqin.com/shuoit/20160427/351759.html)</p>