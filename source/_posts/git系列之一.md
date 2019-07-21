---
title: git系列之一
date: 2019-05-2 16:21:08
categories: 前端
tags: 
    - git
    - 版本控制工具
---
### 前记
记录一下git在工作中常见得一些操作,总结一下，方便今后工作中查询
### 使用
想要用git得话，你必须在一个版本库里面，那么怎么才可以有一个版本库了，有两种方法在首先接收第一种
git clone 这个是克隆一个远程得库生成一个本地库，跟随着这个远程库一起生成得有.git文件，这个就是git得版本库，这里我们用一张图看一下版本库几个概念
![image.png](https://upload-images.jianshu.io/upload_images/13805935-78c35bd189557688.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
工作区：工作去就是你克隆下来得代码目录那一块，是你新增，修改代码得地方
暂存区：暂存区在你得版本库里面，在你使用git add 得时候，git会把你工作区得修改，放到暂存区里面去
本地分支：当你使用git commit 得时候，这个命令是把暂存区得代码提交到本地分支上，注意是本地的分支
介绍了上面三分名词，前两个可以理解，我们看看第三个，本地得分支
> git clone -b develop https://github.com/yizhengfeng-jj/test.git
克隆远程的某一个指定分支

### 本地分支
跟随着版本库一起被克隆下来得还有远程分支
![image.png](https://upload-images.jianshu.io/upload_images/13805935-3da772aef8e51a89.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
红色的表示远程分支，那么绿色的master是什么了，他是跟踪远程仓库中活动分支的本地分支，是git自己创建的
### 创建版本库
上面说了用git clone 可以生成一个版本库，第二个方法是自己本地创建一个，创建命令是
git init 这个命令就是初始化一个版本库，生成一个.git文件，此时版本库创建好了我们怎么把他推上远程分支了，假如我们得git客户端是github，你创建一个私有仓库就知道了，看下面得照片
![image.png](https://upload-images.jianshu.io/upload_images/13805935-fdb066a9af96ba33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
第一步：git init 初始化版本库
第二步：git add . 把所有得更改放在暂存区
第三步：git remote add origin xxxxxx 和远程库关联
第四步：git push -u origin master 把本地得master 推到远程去，刚创建时候只有一个master分支，所以这个命令就是推送你得整个版本库

!! 如果第三步不小心关联错了，可以用下面命令取消关联
> git remote remove origin