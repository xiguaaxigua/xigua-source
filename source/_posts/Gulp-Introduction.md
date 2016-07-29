---
title: gulp学习笔记
date: 2016-07-28 21:25:45
tags: [JavaScript, 工具]
---

gulp从拼写到精通。
<!-- more -->

![](http://7xtoaz.com1.z0.glb.clouddn.com/glup1.png)

## gulp简介
gulp是前端开发过程中对代码进行构建的工具,是自动化项目的构建利器,使用gulp可以大大的提高工作效率。

gulp是基于node的自动任务运行器,她能自动化的完成javascript/coffee/sass/less/html/image/css等文件的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成,监听文件在改动后自动执行这些任务。

gulp和grunt非常类似,但相比于grunt的频繁IO操作,gulp的流操作能更快更便捷的完成构建任务。

## gulp学习地址
+ [gulp官方地址](http://gulpjs.com)
+ [gulp插件地址](http://gulpjs.com/plugins)
+ [gulp官方API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)
+ [gulp官方API中文](http://www.ydcss.com/archives/424)

## 入门教程
### 全局安装gulp
```
# 博主已从window转到mac开发。
sudo npm install gulp -g

# 作为项目的开发依赖安装
# 全局安装是为了执行gulp任务,本地安装是为了调用gulp插件的功能
sudo npm install --save-dev gulp

# 查看当前gulp的版本号
gulp -v
# CLI version 3.9.1

# 运行gulp
gulp
```

## 新建glup配置文件
在项目根目录下创建名为: `gulpfile.js` 的配置文件:
```
var gulp = require('gulp');

// 默认名为default的task将会默认执行
gulp.task('default', function(){
    // 默认的任务代码
})
```

## 使用gulp自动编译sass的例子

```
var gulp = require('gulp'),
    sass = require('gulp-sass');

// 定义一个compileSass任务
gulp.task('compileSass', function(){
    // 指定gulp操作的文件
    gulp.src('sass/demo.scss')
        // 该任务需要调用的模块
        .pipe(sass())
        // 在css目录中生成demo.scss
        .pipe(gule.dest('css'));
});

// 默认执行compileSass任务
gulp.task('default', ['compileSass']);
```

## gulpAPI

### gulp.scr(globs[, options])
参数:
+ globs:类型String或者Array,所要获取的glob或者包含globs的数组。
+ options:类型Object,通过glob-stream 所传递给 node-glob 的参数。

输出(Emits)符合所提供的匹配模式(glob)或者匹配模式的数组(array of globs)的文件。将返回一个Viny files的stream,他可以为piped到其他的插件中。
```
gulp.src('client/templates/*.jade')
    .pipe(jade())
    .pipe(minify())
    .pipe(gulp.dest('build/minified_templates'));
```

### gulp.dest(path[, options])
参数:
+ path:类型String或者Function,文件将被写入的路径(输出目录),也可以传入一个参数,在函数中返回相应路径,这个函数可以由vinyl文件实例来提供。
+ options:类型Object。

能被pipe进来,并且将会写出文件。并且重新输出(emits)所有数据,因此你可以将它pipe到多个文件夹。如果某文件夹不存在,将会自动创建。
```
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```

### gulp.task(name[, deps], fn)
参数:
+ name:任务的名字,如果你需要在命令行中运行你的某些任务,那么请不要在name中添加空格。
+ deps:类型Array,一个包含任务列表的数组,这些任务会在你当前任务运行之前完成。注意:你的任务是否在这些前置依赖的任务完成之前运行了?请一定要确保你所依赖的任务列表中的任务都使用了正确的异步执行方式:使用一个callback,或者返回一个promise或stream。
    ```
    gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
      // 做一些事
    });
    ```
+ fn:该函数定义任务所要执行的一些操作。通常来说,它会是这种形式:
    ```
    gulp.src().pipe(somePlugin());
    ```

### gulp.watch(glob [, opts], tasks) 或 gulp.watch(glob [, opts, cb])
监视文件,并且在文件发生改动的时候做一些事情,它总会返回一个EventEmitter来发射(emit)change事件。

### gulp.watch(glob[, opts], tasks)
参数:
+ glob:一个glob字符串,或者一个包含多个glob的数组,用来指定具体监控那些文件的变动。
+ opts:传给gaze的参数。
+ tasks:需要在文件变动之后执行的一个或者多个通过glup.task()创建的task的名字。
    ```
    var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
    watcher.on('change', function(event) {
      console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
    });
    ```

### gulp.watch(glob[, opts, cb])
参数:
+ globs,同上。
+ opts,同上。
+ cb:每次变动需要执行的callback。callback会被传入一个名为event的对象。这个对象描述了所监控到的变动。
    ```
    gulp.watch('js/**/*.js', function(event) {
      console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
    });
    ```

## 使用webStrom运行gulp任务





















