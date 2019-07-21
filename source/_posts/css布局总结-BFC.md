---
title: css布局总结--BFC
date: 2018-12-21 15:50:51
categories: 前端
tags:
    - css
    - 布局
---
## 前言
以前对BFC有所了解，但是很久不看，就有些遗忘了，这次写一个总结，帮助自己回忆一下相关的内容，以及做为今后查阅资料的地方

### 盒模型
在说BFC之前，首先说说CSS中最常见的盒子类型
block box level 块级盒子：display属性为block, list-item, table时候触发
inline box level 行内盒子：display属性为inline, inline-block,inline-table的时候触发
run in box css3规范中定义,[run-in-box](https://www.zhangxinxu.com/wordpress/2012/03/tip-css-multiline-display/)

### BFC定义
BFC---意思是块极格式化上下文，拥有自己的一套渲染的逻辑，它决定了自己的子元素的布局方式，以及自己和其他元素的渲染关系

### BFC的触发条件
1：float属性不为none
2：overflow属性不为visible
3：position的属性为absolute或者fixed
4：display属性为inline-block,table-cell,table-caption,flex,inline-flex

### BFC的特点
1：BFC内部的相邻的box之间的margin会相互重叠
2：BFC内部所有的box的左margin都会和BFC左border对齐
3：BFC区域不会和float box重叠
4：计算BFC高度的时候，内部的float box也会计算在内

### BFC的应用场景
#### 自适应的两栏布局
```css
.test {
            height: 300px;
            width: 300px;
            border: solid 1px red;
        }
        .test1 {
            width: 100px;
            height: 80px;
            background: red;
            float: left;
        }
        .test2 {
            height: 100px;
            background: pink;
            
        }
```
```html
<div class='test'>
    <div class='test1'></div>
    <div class='test2'></div>
</div>
```
效果图
![image.png](https://upload-images.jianshu.io/upload_images/13805935-ae87d29bf789515e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
原因：因为第一个div的是float,所以浮动到上去了，压在了第二个的div上面
解决方法： 我们可以根据BFC的特点的第三点，可以设置第二个div为一个新的一个BFC，那么就不会和第一个div进行重叠.
```css
.test {
            height: 300px;
            width: 300px;
            border: solid 1px red;
        }
        .test1 {
            width: 100px;
            height: 80px;
            background: red;
            float: left;
        }
        .test2 {
            height: 100px;
            background: pink;
            overflow: hidden;
        }
```
![image.png](https://upload-images.jianshu.io/upload_images/13805935-60bdfd1c709c21d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 清除浮动
```css
.test {
            width: 300px;
            border: solid 1px red;
        }
        .test1 {
            width: 100px;
            height: 80px;
            background: red;
            float: left;
        }
        .test2 {
            height: 100px;
            background: pink;
            float: left;
        }
```
```html
<div class='test'>
    <div class='test1'></div>
    <div class='test2'></div>
</div>
```
![image.png](https://upload-images.jianshu.io/upload_images/13805935-72ebd39cc4b2d10b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
原因：因为两个div都是浮动类型，所以高度坍塌
解决方法：根据BFC的第四条规则，计算高度的时候，浮动元素也会被计算在内,那么直接给test这个类设置一个overflow:hidden触发BFC即可

![image.png](https://upload-images.jianshu.io/upload_images/13805935-35da6bb77b74ed56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 防止垂直的margin重叠
```css
.test {
            width: 300px;
            border: solid 1px red;
            overflow: hidden;
        }
        .test1 {
            width: 100px;
            height: 80px;
            background: red;
            margin:20px;
        }
        .test2 {
            height: 100px;
            background: pink;
             margin:20px;
        }
```
![image.png](https://upload-images.jianshu.io/upload_images/13805935-b450785aa8e5380a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
原因：BFC内部相邻的box的margin会重叠
解决方法：重新建立一个新的BFC，这样里面的P和外面的P就不是在一个BFC里面了
```css
.wrap {overflow: hidden;}
```
```html
<div class='test'>
    <div class='test1'></div>
    <div class='wrap'>
        <p class='test2'></p>
    </div>
</div
```
![image.png](https://upload-images.jianshu.io/upload_images/13805935-648aec85cc581466.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 防止文字环绕
```css
 .test {
            width: 300px;
            border: solid 1px red;
            overflow: hidden;
        }
        .test1 {
            width: 100px;
            height: 80px;
            background: red;
            float: left;
        }
        .test2 {background: pink; }
```
```html
<div class='test'>
    <div class='test1'></div>
    <div class='test2'>你好嗨你好你啊上帝爱上决定啊你好嗨你好你啊上帝爱上决定啊你好嗨你好你啊上帝爱上决定啊你好嗨你好你啊上帝爱上决定啊你好嗨你好你啊上帝爱上决定啊你好嗨你好你啊上帝爱上决定啊你好嗨你好你啊上帝爱上决定啊</div>
</div>
```
![image.png](https://upload-images.jianshu.io/upload_images/13805935-eed78bce3864683e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
原因：因为BFC特性，里面的每一个box的margin都要和BFC的border重叠。所以文字围绕
解决方法：让第二个div拥有自己的BFC那么就不会受父亲的限制
```
.test2 {
            background: pink;
            overflow: hidden;
            }
```
![image.png](https://upload-images.jianshu.io/upload_images/13805935-f7cb6a72360c0648.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





