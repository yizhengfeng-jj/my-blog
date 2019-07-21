---
title: css 选择器
date: 2019-04-28 16:19:23
categories: 前端
tags:
    - css
---
### 前记
css 选择器有很多，我们平时用的相对较少，所以有时候一些问题了解的不是很清楚，所以我自己学习总结总结一下，加深印象
#### :first-child
w3c解释：选择属于父元素的第一个子元素的每个 <p> 元素
MDN解释: 表示在一组兄弟元素中的第一个元素。
但是感觉都有点解释不好，意思不清楚，所以说自己总结了一下
含义：指定范围内，指定目标父元素中，第一个子元素是指定元素时有效!
```html
p:first-child {
    color: red;
}
// 此时不生效
<span>aa</span>
<p>aa</p>

// 生效
<p>aa</p>
<p>aa</p>
```

#### : last-child
含义： 指定范围内，指定目标父元素中，最后一个子元素是指定元素时有效!
```html
p:first-child {
    color: red;
}
// 生效
<span>aa</span>
<p>aa</p>

// 不生效
<p>aa</p>
<span>aa</span>
```
#### :first-of-type
含义： 指定范围内，指定目标父元素中，是第一个指定目标时有效!
这个选择的侧重点时只要有就可以了，找到第一个，而不是说第一个子元素必须时指定元素
```html
p:first-child {
    color: red;
}
// 生效
<span>aa</span>
<p>aa</p>

// 生效
<p>aa</p>
<span>aa</span>
```
#### :last-of-type 
含义： 指定范围内，指定目标父元素中，是倒数第一个指定目标时有效!
这个和上面的时一样的，只不过时从后面开始找

#### :only-of-type
含义：指定范围内，指定目标父元素中， 指定元素是独一无二时有效!
```html
main :only-of-type {
  color: red;
}
<main>
  <div>I am `div` #1.</div>
  <p>I am the only `p` among my siblings.</p>
  <div>I am `div` #2.</div>
  <div>I am `div` #3.
    <i>I am the only `i` child.</i>
    <em>I am `em` #1.</em>
    <em>I am `em` #2.</em>
  </div>
</main>
```
![image.png](https://upload-images.jianshu.io/upload_images/13805935-30b451501bba6a84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
！！！注意main后面有一个空格，表示时main里面所有的
p和i标签在他们的副元素中时独一无二的所以样式生效

#### ：only-child
含义：指定范围内，指定目标父元素中， 指定元素是唯一一个子元素时有效!
意思时父元素只有这一个元素
```html
div :only-child {
  color: red;
}

// 生效
<div><p>aaa</p></div>

//  不生效
<div><p>aaa</p><span>bbb</span></div>
```
#### ：nth-child(n)
含义：指定范围内，指定目标父元素中， 第n个位置是指定元素时有效,其中n=0, 1, 2!nth-child是从1开始的，意思是nth-child(1),才表示第一个

#### :nth-last-child(n)
含义：指定范围内，指定目标父元素中， 倒数第n个位置是指定元素时有效,其中n=0, 1, 2!nth-child是从1开始的，意思是nth-child(1),才表示倒数第一个

#### :nth-of-type(n)
含义：指定范围内，指定目标父元素中， 是第n个指定元素时有效,其中n=0, 1, 2 !nth-of-type是从1开始的，意思是nth-of-type(1),才表示第一个

#### :nth-last-of-type(n)
含义：指定范围内，指定目标父元素中， 是倒数第n个指定元素时有效,其中n=0, 1, 2 !nth-last-of-type是从1开始的，意思是nth-last-of-type(1),才表示单数第一个