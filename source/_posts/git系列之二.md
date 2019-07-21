---
title: git系列之二
date: 2019-05-2 16:21:08
categories: 前端
tags: 
    - git
    - 版本控制工具
---
### 前记
上一篇主要介绍了git一些概念，这篇主要讲一下git得常见命令
git status：查看工作区得状态，我们经常用这个命令查看
git add ： 把工作区得修改放到暂存区里面去，
git commit -m ''：把暂存区里面得修改，提交到本地库的分支上去

git branch：查看当前分支是哪个
git checkout : 切换分支
git log : 查看commit得记录

### 场景1
git 中比较重要得一个命令就是回退
当你修改了代码，并且用git add . 之后发现更改得文件有问题，你想改回来，这个时候你可以直接去修改一次，然后在git add . 一次就行，但是如果你git add . 之后并且还git commit -m ''; 此时得工作区是干净得，你想撤回之前得操作怎么办，那么就用到了git reset  --hard HEAD^
HEAD：当前检出记录的符号引用，默认指向最新得提交
HEAD\^：^表示得是上一个，这个命令就是表示上一次得提交
HEAD\~2：~可以让你指定数字，指定回退几次
--hard：--hard 这个要注意，一旦你加了--hard得话，你返回到上一次得提交，但是你工作区得修改全部没了。

### 场景２
git中还有一个比较重要得场景是代码回滚，就是突然一个出现了一个bug，但是不知道这个bug是哪次提交产生得，那么就可以用git checkout 123aa
git checkout 一般用来切换分支的，但是如果一旦在当前分支用这个命令，指定提交得hash值得话，就是让HEAD分离（默认是HEAD和当前得分支是指定同一个最新提交，所有当前分支总是有一个*号），一但HEAD分离，那么HEAD指向了其他得提交，那么你当前分支得代码，就是HEAD指向得代码，利用这个特性我们就可以做到代码得回滚了

### 场景3
我们在实际应用git操作中，用得最多得可能是代码合并，代码合并有两种一种是 
git merge Bug ：当前分支合并bug，这个情况是bug分支和当前分支是独立得，很好得追踪提交得历史，但是会让代码线比较复杂，看下面一个图
![image.png](https://upload-images.jianshu.io/upload_images/13805935-aea43fc36c9dc12b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
第二种合并分支的方法是 git rebase。Rebase 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去
Rebase 的优势就是可以创造更线性的提交历史。如果只允许使用 Rebase 的话，代码库的提交历史将会变得异常清晰。
git rebase master 把当前分支提交内容移动到master分支上去
![image.png](https://upload-images.jianshu.io/upload_images/13805935-48d84b3c54ae902d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这里要注意一下，此时master分支不是最新的，bugFix才是，和merge不同，此时你还要把master跟新
git checkout master
git rebase bugFix
或者git rebase bugFix master 
### 场景4
比如发版得时候我们有一个release分支，一个develop分支，我们在develop上面提交一个功能，但是产品临时说也想发出去，那我们怎么办，直接把develop代码复制过来么，我们可以有更简单得方法
git checkout release
git cherry-pick C4 这个命令就是把某一个分支得提交，复制一份放到当前分支，C4是某个提交得hash值，一般都是把其他得分支提交拿过来，记住只是把C4这次提交得内容拿过来

###场景５
如果是回滚还是代码得撤销，我们都需要知道提交得hash值，所以会用到git log,有时候这样会有点复杂，因为都是hash，如果commit 信息写得不好，一时还不知道是哪个，此时我们就可以使用git tag 来打一个标签，用来设置某一个提交得id，快速切换到此次得提交
git tag -a 3.0.8 -m '3.0.8提交'
-a：增加标签
3.0.8：标签名,
-m：标签得描述信息
git tags 查看所有的标签
git push origin <tagname> 推送一个本地得标签
git push origin --tags 推送本地所有得标签到远程
git tag -d <tagname> 删除本地某一个分支
git push origin :refs/tags/<tagname> 删除一个远程分支
