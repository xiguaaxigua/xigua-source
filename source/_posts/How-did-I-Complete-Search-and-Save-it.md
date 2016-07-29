---
title: 我是怎么实现搜索补全、搜索历史的
date: 2016-06-25 16:03:40
tags: [jQuery]
---
记录工作中的一个需求。
<!--more-->

### 胡说八道
> 你可以看不起一个学渣，但是你不能看不起他强烈的求知欲！
<p style="text-align: right">——Aisin Gioro Da Vinci</p>

### 需求
整个功能的具体需求如下：

+ 1.在搜索框输入关键字以后，显示关键字联想。
+ 2.显示分类搜索。
+ 3.用户点击搜索后，记录搜索历史。
+ 4.用户再次搜索时，显示历史记录。
+ 5.历史记录存储4条。
+ 6.不能重复记录搜索记录。

UI妹子给的设计图：

![](http://7xtoaz.com1.z0.glb.clouddn.com/search01.png)

写的demo效果，演示而已，难免有出入(⊙ˍ⊙)：

![](http://7xtoaz.com1.z0.glb.clouddn.com/search03.png)

### 实现的具体思路

+ 1.使用到了jQuery的autocomplete插件。
+ 2.历史记录以数组的形式存储在localStorage中。

目录结构：

![](http://7xtoaz.com1.z0.glb.clouddn.com/search02.png)

### 代码部分

期待与你交流...

### data.json
```
[
  "ActionScript",
  "AppleScript",
  "Asp",
  "BASIC",
  "C",
  "C++",
  "Clojure"
]
```

### demo.html
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>搜索Demo</title>
	<link rel="stylesheet" href="jquery-ui.css">
	<link rel="stylesheet" href="demo.css">
	
	<script src="jquery.js"></script>
	<script src="jquery-ui.js"></script>
	<script src="demo.js"></script>
</head>
<body>
	<div class="container clearfix">
		<div class="search-box">
			<input type="text" id="search-input">
		</div>
		<div class="btn-box" id="search-btn">搜索</div>
		<div class="history"></div>
	</div>
</body>
</html>
```

### demo.css
```
/*reset*/
body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{
    margin:0;padding:0;}
.clear{ background: none; border: 0; clear: both; margin: 0; padding: 0;}
.clearfix:after { content: "."; display: block;height: 0; clear: both; visibility: hidden;}
.clearfix{*+height:1%;}
h1, h2, h3, h4, h5, h6, p, ul, li, dl, dt, dd {font-size:100%; font-weight: normal; margin:0px; padding:0px; list-style:none;}
table{ border-collapse:collapse;border-spacing:0;}
img,fieldset{ border:0; vertical-align:middle;}
*:focus{ outline:0;}
input.button,button{ cursor:pointer;overflow:visible;}
textarea{ overflow:auto;}

.container
{
    position: relative;

    width: 500px;
    margin: 10px auto 0 auto;

    border: 1px solid #000;
}
.search-box,
.btn-box
{
    line-height: 30px;

    float: left;

    height: 30px;

    border: none;
}
.search-box
{
    position: relative;

    width: 400px;
}
.search-box input
{
    position: absolute;
    top: 0;
    left: 0;

    width: 100%;
    height: 100%;

    border: none;
}
.btn-box
{
    width: 100px;

    cursor: pointer;
    text-align: center;

    color: #fff;
    background: #000;
}

/*自定义searchList样式*/
#searchList
{
    width: 499px;
    padding: 0 0 15px 0;

    list-style-image: none;

    border: 1px solid #ccc;
    border-radius: 0;
}
#searchList li
{
    cursor: pointer;
}
#searchList li a
{
    color: #777;
}
#searchList li:first-child
{
    cursor: default;
}
#searchList li:first-child:hover
{
    background: none;
}
#searchList li:first-child a
{
    font-size: 13px;

    color: #000;
}
#searchList li:hover
{
    line-height: 30px;

    border-radius: 0;
    background: #eee;
}
#searchList li a
{
    font-size: 14px;

    margin: 0;

    border: none!important;
    background: none!important;
}
#searchList li a span
{
    color: #009c4c;
}
#searchList li.hr
{
    width: 100%;
    height: 1px;
    margin: 0;
    margin: 8px 0 4px 0;
    padding: 0;

    background: #ccc;
}

/*历史记录*/
.history
{
    font-size: 14px;

    position: absolute;
    top: 31px;
    left: -1px;

    display: none;

    width: 500px;
    padding: 0;

    border: 1px solid #ccc;
    border-radius: 0;
}
.history li
{
    width: 100%;
    margin: 0;
    padding: 0;
}
.history li a
{
    font-weight: normal;
    line-height: 1.5;

    display: block;

    min-height: 0;
    padding: 2px .4em;

    text-decoration: none;

    color: #777;
}
.history li:first-child
{
    cursor: default;
}
.history li:first-child:hover
{
    background: none;
}
.history li:first-child a
{
    font-size: 13px;

    color: #000;
}
.history li:hover
{
    line-height: 30px;

    border-radius: 0;
    background: #eee;
}
```

### demo.js
```
;$(function(){
	"use strict";

	//搜索框默认的文字提示
	var _defaultSearchVal = "搜点什么吧...";

	//数据源的路径
	var _dataJsonRoot = "data.json";

	//模拟分类搜索的数据
	var cate = { keywords: ["香蕉、草莓、番茄酱、芭芭拉辣酱", "零食", "小吃"] };

	//autocomplete部分
	$("#search-input")
		.attr("placeholder", _defaultSearchVal)
		.focus(function(){
			// console.log('Focusing!');
			if($(this).val() == ''){
				$(this).attr("placeholder", '');				
			}
			if($(this).val() == '' || $(this).val() == _defaultSearchVal){
				// console.log('Show History!');
				showSearchInputHistory();
			}else{
				hideSearchInputHistory();
			}
		})
		.blur(function(){
			if($(this).val() == ''){
				$(this).attr("placeholder", _defaultSearchVal);
				// console.log('Show History!');
				hideSearchInputHistory();
			}
		})
		.autocomplete({
			//autocomplete需要的数据源，即data.json
			source: function(req, res){
				$.getJSON(_dataJsonRoot, function(json){
					// console.log(json);
					res(json);
				});
			},

			//autocomplete激活时的事件
			open: function(){
				resizeSearchList();
			},

			//autocomplete结束时的事件
			close: function(){

			},

			//用户激活autocomplete需要的最少字符数目
			minLength:1,

			//用户选中autocomplete生成的数据时的事件
			select: function(event, ui){
				// console.log(ui.item.value);
				console.log('Saving data...');
				saveSearchInputHistory(ui.item.value);

			}

		})

		//搜索框获取焦点
		.focus(function(req, res){
            //$.trim用于去掉字符串首尾的空白字符
            var userInput = $.trim($("#search-input").val());
            if(userInput !== _defaultSearchVal){
                $(this).autocomplete("search");
            }
        })

		//自定义显示样式
		.data( "ui-autocomplete" )._renderItem = function( ul, item ) {
			ul.attr("id", "searchList");
			var o = $("#searchList").offset();
	      	return $( "<li></li>" )
		        .append( "<a>" + item.label + "</a>" )
		        .appendTo( ul );
	    };

	//点击搜索按钮
	$("#search-btn").click(function(){
		saveSearchInputHistory($("#search-input").val());
	});

	//为了和UI差不多，需要修改autocomplete默认的样式
	function resizeSearchList(cate){
		var userInput = $.trim($("#search-input").val());
		var temp_str = '';
		temp_str = '<li class="ui-menu-item" role="presentation"><a class="ui-corner-all">' + userInput + '</a></li>';
		for (var i = 0; i < cate.keywords.length; i++) {
            temp_str += '<li class="ui-menu-item" role="presentation"><a style="text-indent: 1em;" class="ui-corner-all" href="url">在<span>' + cate.keywords[i] + '</span>中搜索</a></li>';
        }
        temp_str += '<li class="hr"></li>';
        $("#searchList").prepend(temp_str);

    	//如果复杂的话可以写个function
        var ulLeft = $("#searchList").css("left");
        ulLeft = ulLeft.replace("px", "");       
        ulLeft = Number(ulLeft) - 1 + 'px';
        $("#searchList").css("left", ulLeft);

        var ulTop = $("#searchList").css("top");
        ulTop = ulTop.replace("px", "");       
        ulTop = Number(ulTop) + 1 + 'px';
        $("#searchList").css("top", ulTop);
	}

	//存储搜索记录
	function saveSearchInputHistory(keywords) {
	    if (window.localStorage) {
	        var temp_arr = ('searchHistory' in window.localStorage) ? window.localStorage.searchHistory : '';
	        temp_arr = temp_arr.split(",");
	        if (!temp_arr) {
	            //无记录
	            temp_arr.push(keywords);
	        } else {
	            //有记录
	            if (temp_arr.indexOf(keywords) == -1) {
	                temp_arr.push(keywords);
	            }
	        }
	        temp_arr.splice(0, temp_arr.length - 4);
	        window.localStorage.searchHistory = temp_arr;
	    }
	}

	//展示搜索记录
	function showSearchInputHistory() {
		// console.log('Reading History');
	    var s_temp_arr = ('searchHistory' in window.localStorage) ? window.localStorage.searchHistory : ',';
	    s_temp_arr = s_temp_arr.substr(1).split(",").reverse();
	    
		var temp_str = '';
		if(('searchHistory' in window.localStorage)){
			temp_str += '<li><a>搜索历史</a></li>';
		}else{
			temp_str += '<li><a>暂无历史</a></li>';
		}

	    for (var i = 0; i < s_temp_arr.length; i++) {	    	
	        temp_str += '<li><a href="url">' + s_temp_arr[i] + '</a></li>';
	    }
	    // console.log(temp_str);
	    $(".history").html(temp_str);
	    $(".history").show();
	}

	//隐藏搜索记录
	function hideSearchInputHistory(){
		$(".history").hide();
	}
});
```

### END
关于data.json：

用户键入关键字后，应该对从后台拿到的数据做处理，demo中没有加此功能，另外如果数据太长的话，页面的滚动条会对效果造成一定的影响，故此demo中data.json的数据较少，自行研究吧。

最后放个压缩包吧，万一有人需要呢？[点击下载](http://pan.baidu.com/s/1slH04xf)（百度云，174KB，密码：lyuo）。
