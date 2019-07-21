---
title: 减少if-else的使用
date: 2019-03-16 16:02:29
categories: 前端
tags:
    - js
---
## 前言
之前写了很多代码，后来看看，发现里面有很多的if-else的嵌套，代码都是平铺直叙的，即使是很简单的逻辑，用了过多嵌套的if-else的话，也会感觉一下字子看不出这段代码写的是什么逻辑，所以能减少if-else嵌套是很很有必要的。下面来看看我在网上查询资料后，所总结的如果减少if-else的方法。
## 优化方法
1：简单的判断尽量用&&和||或者三元来展示
2：合并表达式
3：写完重新看下逻辑，扁平化if-else嵌套，平行显示
4：有时候不一定从正向逻辑考虑，考虑反向逻辑试试
5：有时候switch-case看起来也比if-else好
6：用面向对象的思想，减少多状态的if-else

## 代码讲解
### 简单的判断尽量用&&和||或者三元来展示
```javascript
if (a) {
a = a;
}else {
 a = b;
}
// 优化之后
a = a || b;
====================
if (a > 0) {
  b = '大于0';
}else {
  b = '小于等于0';
}
// 优化之后
b = a > 0 ? '大于0' : '小于等于0';
```
### 合并表达式
```jacvascript 
if (data.status === 200) {
  if (data.data.length !== 0) {
    result = data.data;
}
}
// 优化后
if (data.status === 200 && data.data.length !== 0) {
  result = data.data;
}

=================
if (isNaN(a)) {
  return 0;
}

if (a < 0) {
  return 0;
}
// 优化后
if (a <0 && isNaN(a)) {
    return 0;
}
```
### 写完重新看下逻辑，扁平化if-else嵌套，平行显示
```javascript 
 function getData() {
  let result;
  
if (a <= 0 ) {
   result = 'a小于等于0';
}else{
    if (a >0 && a <= 10) {
      result = 'a在0到10之间';
  }else {
      if (a % 2 === 0) {
       result = 'a是偶数' ;
      }else {
                result = 'a不是偶数';
       }
}
}
return result;
}

// 这样写的这个逻辑是平铺直叙的，我们经常写这样的代码，我们想减少这样的if-else的时候，
// 我们就要写完好好想想我们的逻辑，结合扁平化思想，平行改写上面代码
================
a <= 10    => 'a小于等于0'
0 < a <= 10 => 'a在0到10之间'
a >10 并且 除以2为0 => 'a是偶数'
最后'a不是偶数'
================
//按照这个逻辑我们改写以上代码
if (a <=0) {
  return  'a小于等于0';
}

if (a > 0 && a <= 10) {
 return  = 'a在0到10之间';
}
if (a % 2 === 0) {
  return  'a是偶数';
}
return 'a不是偶数';
以上代码重新思考了逻辑，结合扁平化思想，改成这样
 ```
### 有时候不一定从正向逻辑考虑，考虑反向逻辑试试
加入有以下场景，写一个计算蔬菜价格的函数如下
![image.png](https://upload-images.jianshu.io/upload_images/13805935-584bfa065db4ca02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们暂定蔬菜不能为空，价格和树龄大于0，写一个计算价格的函数
```javascript
let caculateNum = (item, total, price) => {
   let result = ‘输入有误’;
    if (item) {
     if (toatl > 0 && price > 0) {
       result = total * price
   }
}
return result;
}
```
这样写没有问题，我们想要减少if-else嵌套，可以合并条件表达式，但是我们这次想用条件取反试试
```javascript
let caculateNum = (item, total, price) => {
   let result = ‘输入有误’;
   
   if (!item) {
    return  ‘输入有误’;
}
 if (toatl > 0 && price > 0) {
       result = total * price
   }

}
return result;
}
```
这样子反而多了，我们可以继续条件取反
```jacvaript
let caculateNum = (item, total, price) => {
   let result = ‘输入有误’;
   
   if (!item) {
    return  '输入有误';
}
 if (toatl <= 0 || price <= 0) {
       return  '输入有误';
   }
return  total * price;
}

// 上面依旧可以改写，合并表达式
let caculateNum = (item, total, price) => {
   let result = '输入有误';
   
   if (!item || toatl <= 0 || price <= 0) {
    return  '输入有误';
}
return  total * price;
}
```
### 有时候switch-case看起来也比if-else好
```jacascript
let getData = type => {
    if (type === 'success') {
    console.log('success')
}else if (type === 'fail') {
 console.log('fail')
}else if (yupe === 'downloding') {
    console.log('downloding')
}
}

// 优化如下
let getData = type => {
   switch (type) {
    case 'success':
    console.log('success')
    break;
   case 'fail':
   console.log('fail')
    break;
   case 'downloding':
    console.log('downloding')
    break;
}
}
```
### 用面向对象的思想，减少多状态的if-else
```javascript
let getData = type => {
    if (type === 'success') {
    console.log('success')
}else if (type === 'fail') {
 console.log('fail')
}else if (yupe === 'downloding') {
    console.log('downloding')
}
}
// 优化之后
let obj = {
    'success': function () {
    console.log('success')
  },
  'fail': function () {
    console.log('fail')
},
'downloding': function () {
     console.log('downloding')
}
}
let getData = type => {
   obj[type];
}

```

