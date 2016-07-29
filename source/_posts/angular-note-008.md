---
title: 008-Angular中的迭代型元素-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-09 16:33:45
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

### 列表、表格、以及其他迭代型元素
ng-repeat可能是最有用的Angular指令了，它可以根据集合中的项目一次创建一组元素的多份拷贝，不管在什么地方，只要你想创建一组事物的列表，你就可以使用这条指令。

比方说，我们正在为老师们编写一套学生花名册系统。我们当然需要从服务器上获取学生信息，但是对于当前这个例子来说，我们还是把模型直接定义在JavaScript代码里面：

```
    var student = [
		{name: "张三", id:'1'},
		{name: "李四", id:'2'},
		{name: "王五", id:'3'}
	];
	function StudentListController($scope){
		$scope.students = students;
	}
```

为了显示这个学生列表，我们可以编写下面的代码：

```
    <ul ng-controller="SutdentListController">
		<li ng-repeat="studnet in students">
			<a href="/sudent/view/{{student.id}}">{{student.name}}</a>
		</li>
	</ul>
```

ng-repeat将会生成标签内部所有HTML元素的一份拷贝，包括放指令的标签。经过这样的操作之后，我们将会看到以下效果：
- 张三
- 李四
- 王五

根据具体情况分别链接到/student/view/1，/student/view/2，/student/view/3。

正如我们前面所看到的，修改学生信息数组将会自动刷新所渲染的列表。如果我们需要向列表中插入一条新的学生信息，可以这样写：

```
	var students = [
		{name: "张三", id:'1'},
		{name: "李四", id:'2'},
		{name: "王五", id:'3'}
	];

	function StudentListController($scope){
		$scope.students = students;

		$scope.insertTom = function(){
			$scope.students.splice(1, 0, {name:'Tom Thumb'}, id: '4');
		}
	}
```

然后添加一个按钮在模板中调用新增的函数：

```
	<ul ng-controller="StudentListController">
		<li ng-repeat="student in students">
			<a href="/student/view/{{student.id}}">{{student.name}}</a>
		</li>	
	</ul>
	<button ng-click="insertTom()">Insert</button>
```

现在我们将会看到：

- 张三
- Tom
- 李四
- 王五

ng-repeat指令可以通过$index返回当前引用的元素符号；还可以通过$first、$middle及$last，ng-repeat指令返回布尔值，告诉你当前元素是否是集合中的第一个元素、中间的元素，或者最后一个元素。

你可能需要使用$index在表格中显示出行号，那么你只要编写下面这样的模板即可：

```
	<table ng-controller="AlbumController">
		<tr ng-repeat="track in album">
			<td>{{$index+1}}</td>
			<td>{{track.name}}</td>
			<td>{{track.duration}}</td>
		</tr>
	</table>
```

对应的控制器代码如下：

```	
	var album = [
		{name: 'Southwest Serenade', duration: '2:34'},
		{name: 'Northern Light Waltz', duration: '3:21'},
		{name: 'Eastern Tango', duration: '17:45'}
	];

	function AlbunController($scope){
		$scope.album = album;
	}
```

然后我们就可以获得以下输出：

- 1 Southwest Serenade 2:34
- 2 Northern Light Waltz 3:21
- 3 Eastern Tango 17:45