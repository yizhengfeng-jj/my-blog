---
title: git系列之三
date: 2019-05-2 16:21:08
categories: 前端
tags: 
    - git
    - 版本控制工具
---
### 前记
这次说一下git里面得远程得相关得操作
#### git fetch
这个命令是把远程库的远程分支代码，拉到本地库的远程分支上，具体我们看看下面的图片
![image.png](https://upload-images.jianshu.io/upload_images/13805935-c102f7ed2b6180fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> 执行了git fetch之后，o/master更新了（这个是本地的远程分支）,但是本地的master分支还没有更新
想要更新本地master 还需要执行git merge o/master

#### git pull 
这个命令也是下拉代码，但是他会自动帮你合并本地的远程分支代码
git pull = git fetch + git merge o/master

#### git push 
这个命令是把本地的修改，推送到远程库的对应的分支，这个命令也是值得说明的
这里主要说明git push参数相关的内容 git push 不带任何参数时的行为与 Git 的一个名为 push.default 的配置有关。它的默认值取决于你正使用的 Git 的版本，所以在推送之前最好检查一下这个配置。
##### git push场景1
我们在本地master分支推送，不加任何参数直接git push 是因为git知道master 关联的是远程库的分支（下面有解释）
##### git push场景2
那要是不知道了，比如我自己本地创建一个分支修改了代码，直接git push 会怎么样，看下图
![image.png](https://upload-images.jianshu.io/upload_images/13805935-62bebfd93a9cd2d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我创建了一个test分支，修改代码,直接git push 提交，他报如下的错，因为你没有指定关联，他不知道要push到远程的哪个分支上去，所以git在bash里面建议你带参数
git push origin source:target
source表示本地分支
target表示远程分支
因为你现在就在test 分支上面了，所以可以直接忽略source
git push origin test 这句话的意思就是把本地的test，推送到远程库的test,但是远程库没有test，怎么办，他会帮你在远程库创建一个远程分支test,所以这也是创建远程分支的一种方法，（另一种直接在git客户端如github创建，然后去关联，下面有讲解）
#### 疑惑
为什么我在本地master分支推送git push，他会把代码推送到远程库的master分支了，难道就是因为master o/master 名字差不多么，真正原因如下
>  master 和 o/master 的关联关系就是由分支的“remote tracking”属性决定的。master 被设定为跟踪 o/master —— 这意味着为 master 分支指定了推送的目的地以及拉取后合并的目标。
你可能想知道 master 分支上这个属性是怎么被设定的，你并没有用任何命令指定过这个属性呀！好吧, 当你克隆仓库的时候, Git 就自动帮你把这个属性设置好了。
当你克隆时, Git 会为远程仓库中的每个分支在本地仓库中创建一个远程分支（比如 o/master）。然后再创建一个跟踪远程仓库中活动分支的本地分支，默认情况下这个本地分支会被命名为 master。
克隆完成后，你会得到一个本地分支（如果没有这个本地分支的话，你的目录就是“空白”的），但是可以查看远程仓库中所有的分支（如果你好奇心很强的话）。这样做对于本地仓库和远程仓库来说，都是最佳选择。

#### 自己设置关联
上面的关联是git默认设置的，那么我们可以自己设置么，答案是可以的
> git checkout -b develop origin/develop

这个命令就是创建一个本地develop分支，并且关联远程库的develop分支，那么这个分支就和远程的一样，这也是我们我们常见的拉取远程分支的方法，并且这是不会合并的
>git branch -u origin/develop develop

这种方法也是创建一个develop分支，并且关联远程库的develop分支，同样是拉取远程分支的一种方式，并且不会合并

> git fetch origin foo

同样的git fetch和git pull 也和上面的git push 一样，拥有参数，但是不同的是
git fetch origin target:source
参数意义反过来了，远程的在前面，本地的在后面,那么上面那句代码的意思是什么了看下图
![image.png](https://upload-images.jianshu.io/upload_images/13805935-1dea4f364617ba12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

他会把远程的foo分支代码，拉到本地的远程分支上，
假如本地没有线上分支怎么办，同样的他会创建一个分支
git fetch origin bar 他会创建一个本地远程分支o/bar，此时能看到的还是master分支，但是你是可以切换到bar上面的
git checkout bar （虽然现在只有o/bar,没有bar分支）的时候，git看到了有o/bar分支，会自动帮你创建一个看的到的bar分支的
![image.png](https://upload-images.jianshu.io/upload_images/13805935-0450ddb4039a0ad5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你想一开始就能看得见，就的把两个参数一起写上去
git fetch origin bar:bar 这样就是创建bar分支，当你push完了，o/bar才会生产
![image.png](https://upload-images.jianshu.io/upload_images/13805935-0d65fc996c725ab4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> git pull origin target:source

git pull 和git fetch是一样的，但是有一个不同的是，他会帮你合并！合并！合并！
git pull origin develop:develop = git fetch origin develop:develop + git merge develop

他在本地创建一个develop, 并且会帮你把develop分支和当前分支合并，如果你是在master分支操作这个命令，那么那就会帮你把develop和master合并
注意！！！
git pull origin develop = git fetch origin develop + git merge o/develop
当命令是一个参数的时候，不同的是本地产生的分支，上面一个直接是看得见的develop，下面这个不带参数的是创建一个o/develop
但是相同的是还是会合并，所以网上说的无论是那种拉取只要你用的是git pull 这个pull命令，但是帮你合并
所以拉取分支不合并的方法，如果上面两种外第三种是git fetch origin develop:develop 是fetch！！


