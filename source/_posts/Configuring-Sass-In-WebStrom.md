---
title: 在WebStrom中配置Sass编译环境
date: 2016-06-23 13:12:21
tags: [工具]
---
Sass的编译。
<!--more-->

### 胡说八道
CSS终究不如SASS方便，就像[SASS官方网站](http://sass-lang.com/)的介绍一样，“Sass: Syntactically Awesome Style Sheets”，风骚的CSS语法！SASS语法可以参考[W3CPlus](http://www.w3cplus.com/sassguide/)。

记录一下在Windows环境的WebStrom中配置SASS编译环境的步骤。

关于WebStrom的配置及其破解汉化等，参照另一篇文章[前端开发利器之WebStrom](http://xiguaaxigua.cn/2016/06/23/An-IDE-Named-WebStrom/)。

### 步骤如下：

1.SASS的安装依赖于Ruby，Ruby的安装请自行百度，也可以参考[Ruby环境搭建](http://www.runoob.com/ruby/ruby-environment.html)。记得勾上环境变量一项，免得麻烦，如果忘了勾选的话，右键计算机，属性里面可以设置。

```
#查看Ruby版本号
ruby -v
```

2.安装SASS，在命令行中（推荐[GitBash](https://git-scm.com/download)）。
常见的Git Bash命令：

```ruby
#克隆别人或者远程共享库的代码
git clone URL

#更新代码
git pull

#查看本地修改与服务器之间的差异
git status

#将差异文件加入版本控制
git add .

#将指定差异文件加入版本控制
git add ReadMe.txt

#将文件提交到本地仓库
git commit -m "注释"

#将本地仓库提交到远程共享库
git push

#将代码提交到主干
git push origin master

#将本地的local合并到服务器的主干
git merge local master

#更改到master库
git checkout master


```

#### git解决冲突：

输入git mergetool后回车，输入解决冲突的工具的名称，直接回车将会打开默认的vimdiff工具，也可以自行配置其他工具（如beyond compare 可以参考[Beyong Compare差异分析工具](http://xiguaaxigua.cn/2016/06/24/Beyond-Compare/)）。


常见的gem（Ruby是一种脚本语言，gem是基于Ruby的一些开发工具包）命令：

```ruby
#更新gem
gem update --system

#从gem源安装gem包
gem install [gemName]

#从本机安装gem包
gem install [gemName].gem

#安装指定版本的gem包
gem install [gemName] --version=[gemVersion]

#更新所有已安装的gem包
gem update

#更新指定的gem包
gem update [gemName]

#卸载指定的gem包，将删除该包所有已经安装的版本
gem uninstall [gemName]

#卸载指定版本的gem包
gem uninstall [gemName] --version=[gemVersion]

#查看本机所有已安装的gem包
gem list [--local]
```

身在墙外，你懂得：

```
gem install sass
```

淘宝镜像：

``` ruby
#依次键入

#移除默认的ruby源
gem sources –r http://rubygems.org/

#添加淘宝的ruby源
gem sources –a http://ruby.taobao.org/

#查看当前的ruby源，请确保只有http://ruby.taobao.org
gem sources –l

#安装sass
gem install sass
```

本地安装如下图：
![](http://www.w3cplus.com/sites/default/files/blogs/2016/1603/install-sass.gif)

当看到以下提示的时候，那么Congratulations！

![](http://7xtoaz.com1.z0.glb.clouddn.com/ConfigingSASS_1.png)

3.打开setting->Tools->file watcher选项。如下图：

![](http://7xtoaz.com1.z0.glb.clouddn.com/ConfigingSASS_2.png)

4.如下图进行配置：

![](http://7xtoaz.com1.z0.glb.clouddn.com/ConfigingSASS_3.png)

一般情况下，只需要修改Arguments和Output paths to refresh，具体情况根据自己的工程进行修改：

```
Arguments:

--style expanded --no-cache --update $FileName$:$ProjectFileDir$/css/$FileNameWithoutExtension$.css

Output paths to refresh:

$ProjectFileDir$/css/$FileNameWithoutExtension$.css
```

5.理论上配置结束，可能有一些奇奇怪怪的小问题，自行百度吧。下边来试一波~

```
body
  background: #eee
  font-size: 12px
p
  background: #0982c1
```

![](http://7xtoaz.com1.z0.glb.clouddn.com/ConfigingSASS_4.png)

果然自动编译了，保存即编译，从此开发效率快了很多呢~~

同样的配置SCSS的编译环境，添加一个SCSS的Watcher，将Program的值改为：ruby下bin目录里的scss.bat即可。
