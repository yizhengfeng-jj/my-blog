---
title: react diff算法和key值作用
date: 2019-07-15 16:43:06
tags:
    - react
    - js
---
### 前言
在知乎看到twobin大神关于react [diff算法](https://zhuanlan.zhihu.com/p/20346379)的总结，我觉得自己有必要理解记录一下，为以后更深入的react diff算法做铺垫

### diff算法
diff算法的使用在我们工作中有多，比如vscode的git管理的文件，可以查看修改哪些地方，软件Beyond Compare 也是基于对文件的比较，甚至redux的扩展工具里面也有对数据的比较等等，但是react如果是基于传统的diff比较，其时间复杂度高达O(n^3)，那么一旦要渲染的节点很多，对性能会有很大的影响，那么react的diff算法做了什么改进了?

### react 中diff算法
在react中diff算法是同级比较的，如下图
![image](https://user-images.githubusercontent.com/20452750/61574159-e952ce80-aaed-11e9-8fbd-e9c18fe62c34.png)
统一颜色的框内部进行比较，当发现红色框内的两个节点不一样，那么react会删除旧集合中的节点，创造新节点，插入dom中。这样只需要遍历一次就可以渲染节点了，如果新旧集合的层级不一样怎么办了，还是一样的比较解决
![image](https://user-images.githubusercontent.com/20452750/61574226-1784de00-aaef-11e9-9b57-6cbef865d472.png)
当遍历到第二层发现没有A了，则把A删除，当遍历到三层，发现他有字节点A，那么就创造A插入在后面

### elment diff
节点比较执行情况分为三种
INSERT_MARKUP：遍历新集合发现里面有的节点在旧集合里面没有，那么创造该节点，插入
MOVE_EXISTING：如果新集合里面的节点在旧集合里面可以找到，那么我们就只需要移动节点就行
REMOVE_NODE：如果旧集合的节点不在新结合里面，则需要删除该节点
看一下下图的比较机制
![image](https://user-images.githubusercontent.com/20452750/61574219-f45a2e80-aaee-11e9-82d8-ac33fc320847.png)
按照之前的比较，新集合的第一个是B，旧集合的是A，发现B！==A那么删除节点A，创造节点B插入，后面的节点一次比较，但是这样的话会造成性能的浪费，因为新旧集合dom都是一样的，就是换了位置，那么这个时候改怎么解决了

### key值的重要性
解决方法就是给每一个dom增加一个唯一的标记key如下图
![image](https://user-images.githubusercontent.com/20452750/61574268-b27db800-aaef-11e9-8886-2b925aa49daa.png)
还是新旧集合的比较，但是比较的时候，先看一下新集合里面的节点在不在老集合里面，如果在只需要移动就行，不在就新增，那么这个机制到底是怎么样子的了，下面借用twobin大神解释来说明
首先对新集合的节点进行循环遍历，for (name in nextChildren)，通过唯一 key 可以判断新老集合中是否存在相同的节点，if (prevChild === nextChild)，如果存在相同节点，则进行移动操作，但在移动前需要将当前节点在老集合中的位置与 lastIndex 进行比较，if (child._mountIndex < lastIndex)，则进行节点移动操作，否则不执行该操作。这是一种顺序优化手段，lastIndex 一直在更新，表示访问过的节点在老集合中最右的位置（即最大的位置），如果新集合中当前访问的节点比 lastIndex 大，说明当前访问节点在老集合中就比上一个节点位置靠后，则该节点不会影响其他节点的位置，因此不用添加到差异队列中，即不执行移动操作，只有当访问的节点比 lastIndex 小时，才需要进行移动操作。

以上图为例，可以更为清晰直观的描述 diff 的差异对比过程：

- 从新集合中取得 B，判断老集合中存在相同节点 B，通过对比节点位置判断是否进行移动操作，B 在老集合中的位置 B._mountIndex = 1，此时 lastIndex = 0，不满足 child._mountIndex < lastIndex 的条件，因此不对 B 进行移动操作；更新 lastIndex = Math.max(prevChild._mountIndex, lastIndex)，其中 prevChild._mountIndex 表示 B 在老集合中的位置，则 lastIndex ＝ 1，并将 B 的位置更新为新集合中的位置prevChild._mountIndex = nextIndex，此时新集合中 B._mountIndex = 0，nextIndex++ 进入下一个节点的判断。

- 从新集合中取得 A，判断老集合中存在相同节点 A，通过对比节点位置判断是否进行移动操作，A 在老集合中的位置 A._mountIndex = 0，此时 lastIndex = 1，满足 child._mountIndex < lastIndex的条件，因此对 A 进行移动操作enqueueMove(this, child._mountIndex, toIndex)，其中 toIndex 其实就是 nextIndex，表示 A 需要移动到的位置；更新 lastIndex = Math.max(prevChild._mountIndex, lastIndex)，则 lastIndex ＝ 1，并将 A 的位置更新为新集合中的位置 prevChild._mountIndex = nextIndex，此时新集合中A._mountIndex = 1，nextIndex++ 进入下一个节点的判断。

- 从新集合中取得 D，判断老集合中存在相同节点 D，通过对比节点位置判断是否进行移动操作，D 在老集合中的位置 D._mountIndex = 3，此时 lastIndex = 1，不满足 child._mountIndex < lastIndex的条件，因此不对 D 进行移动操作；更新 lastIndex = Math.max(prevChild._mountIndex, lastIndex)，则 lastIndex ＝ 3，并将 D 的位置更新为新集合中的位置 prevChild._mountIndex = nextIndex，此时新集合中D._mountIndex = 2，nextIndex++ 进入下一个节点的判断。

- 从新集合中取得 C，判断老集合中存在相同节点 C，通过对比节点位置判断是否进行移动操作，C 在老集合中的位置 C._mountIndex = 2，此时 lastIndex = 3，满足 child._mountIndex < lastIndex 的条件，因此对 C 进行移动操作 enqueueMove(this, child._mountIndex, toIndex)；更新 lastIndex = Math.max(prevChild._mountIndex, lastIndex)，则 lastIndex ＝ 3，并将 C 的位置更新为新集合中的位置 prevChild._mountIndex = nextIndex，此时新集合中 C._mountIndex = 3，nextIndex++ 进入下一个节点的判断，由于 C 已经是最后一个节点，因此 diff 到此完成。

### 总结
根据以上的diff算法的总结我们可以得出以下结论
- key值必须要唯一，这里的唯一指的是相同的dom节点，新旧集合中key值是唯一，所以比较经典问题是key值对好不要是index下标
- dom显示和消失最好是用css控制，而不是移除节点，移除节点的性能消耗更大