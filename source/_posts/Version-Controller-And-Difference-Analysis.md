---
title: 版本控制和差异分析
date: 2016-06-24 22:50:13
tags: [工具, 工作记录]
---
SVN、GIT、BeyondCompare。
<!--more-->

工作中难免要和同事共同开发，版本控制显得尤为重要，博主经常用到的版本控制工具有TortoiseSVN（传说中的“小乌龟”）和GIT，用到的处理代码冲突的工具有Beyond Compare。写到这里，夜深了，突然想起来好久都没有听过歌了，这里放一首Beyond的《海阔天空》吧，

<audio src="http://7xtoaz.com1.z0.glb.clouddn.com/%E6%B5%B7%E9%98%94%E5%A4%A9%E7%A9%BA.mp3" controls></audio>

## 小乌龟
[官网地址](https://tortoisesvn.net/)，很好用的SVN工具，在处理代码冲突的时候比GIT好用。

拉代码的步骤如下：

1.安装成功后如图，打开“TortoiseSVN Repository Brower”。

![](http://7xtoaz.com1.z0.glb.clouddn.com/svn1.png)

2.打开键入你环境的地址，输入你的账号密码即可。

3.待续吧。。。

## 新时代新青年的选择——Git

### Git简介
Git是一个免费的、分布式的版本控制工具，或是一个强调了速度快的源代码工具。

### 基本指令
+ `git clone <url>`克隆远程仓库到本地
+ `git init`初始化
+ `git pull`更新远程仓库到本地
+ `git push`将本地代码上传到远程仓库
+ `git revert`还原一个版本的修改，必须提供一个具体的git版本号
+ `git log`查看历史记录
+ `git commit`提交
+ `git rm`从当前的工作空间中删除文件
+ `git add`添加文件到缓存区


### 基本指令
+ `git config --global user.name "Your Name"`
+ `git config --global user.email "by_openwater@163.com"`
+ `git config --global core.editor Sublime`
+ `git init`初始化一个新仓库
+ `git help <command>`获取某个指令的帮助信息

### Commit指令
commit的时候发生了什么？
![](https://i.imgur.com/B0w11nb.png)

Working Directory开发目录
Staging Area缓存区
Repository远程仓库

+ `git add Demo.html`添加一个文件到缓存区
+ `git add .`添加所有文件到缓存区
+ `git add *.js`添加所有的js文件到缓存区
+ `git rm --cached Demo.html`删除缓存区中的`demo.html`文件
+ `git commit -m "Your Notes"`提交文件并注释
+ `git log`查看提交记录

### 缓存区指令
+ `git reset HEAD <fileName>`从缓存区删除一个文件
+ `git reset HEAD`清空缓存区
+ `git checkout <filename>`撤回对该文件的修改（并从缓存区删除该文件）
+ `git commit -m "A demo commit" --amend`添加/修改在缓存区的最后一次提交的文件
+ `git commit -v -a --amend`添加/修改在缓存区的最后一次提交的文件
+ .gitignore 设置不想被git管理的文件
+ `git diff <filename>`查看当前文件与仓库中文件的差异
+ `git diff`查看所有文件与仓库中文件的差异
+ `git reset HEAD~2 --soft`从仓库中删除最近两次提交，但是不舍弃这些文件的更改
+ `git reset HEAD~2 --hard`从仓库中删除最近两次提价，并舍弃这些文件的更改
+ `git reset <commit> --soft --hard`
    + `--soft`将所有被更改的文件复原到“待提交”的状态
    + `--hard`commit之后，对被git管理的文件的任何更改都被丢弃
+ `git reflog`显示所有commit（包括被删除的）
+ `git merge <commit hash>`重新commit
+ `git clean -f`删除开发目录中没有被git管理的文件

### 储藏区指令
+ `git stash`将缓存区的文件移动到储藏区（类似于另一种类型的工作区）
+ `git stash list`查看储藏队列
+ `git stash apply`将最近一次储藏恢复到缓存区
+ `git stash clean`清空储藏队列
+ `git stash save "Name of Stash"`命名一个储藏区
+ `git stash pop`将最近一次储藏恢复到缓存区并从储藏队列删除此储藏
+ `git stash drop`从储藏队列删除最后一次储藏
+ `git stash drop stash@{2}`从储藏队列删除指定储藏

### 分支指令，对本质的增、删、查等操作。
+ `git branch <branchName>`创建一个名为branchName分支（将切换为当前分支）
+ `git branch`查看所有分支
+ `git checkout master`切换到主分支
+ `git merge <branch>`合并分支
+ `git rebase master`现将master上的更改合并到当前分支，再添加当前分支的更改，如果有冲突，解决冲突后加`--continue`参数继续合并
+ `git branch -d <branch>`删除分支，-D则强制删除分支
+ `git merge <branch> --squash`将多次提价合并成一个，其流程如下：

```
# Go to the `master` branch
git checkout master

# Create a temp branch
git checkout -b temp

# Merge the feature/x branch into the temp using --squash
git merge feature/x --squash

# See the new modifications/files in the Staging Area
git status

# Create the unified commit
git commit -m "Add feature/x"

# Delete the feature/x branch
git branch -D feature/x
```

rebase和merge的区别：
+ rebase：
    + 提交历史是线性的
    + 缺点：会删除最近一个commit，然后创建一次新的commit
    + 如果已提交到远程，不要使用rebase
+ merge：
    + 提交历史是分叉的
    + 对于两个分支的合并，会创建一次新的commit

### 远程仓库管理
+ `git remote add <name> <url>`添加一个将被追踪的远程仓库
+ `git remote rm <name>`移除一个远程仓库
+ `git push <remote> <remote-branch>`将当前分支的本地commit推送到远程仓库
+ `git fetch <remote> <remote-branch>`拉取远程仓库的最新commit到当前分支，不会合并
+ `git pull <remote> <remote-branch>`拉取远程仓库的最新commit到当前分支，并自动merge
+ `git pull --rebase`以rebase的方式进行合并，而不是merge

### 其他有用的命令
+ `git tag <name>`创建一个tag
+ `git push --tags`将本地tags推送到远程仓库
+ `git push <tags>`推送到指定的本地tag到远程


## 冲突分析工具——Beyond Compare

