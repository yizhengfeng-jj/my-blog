---
title: Promise小谈
date: 2019-4-18 16:10:18
categories: 前端
tags:
    - js
---
## 前言
看完阮一峰大神的Promise详解之后，自己总结一下promise的用法，毕竟看的东西是别人的，自己写的总结的东西是自己的，文中大部分是根据阮一峰大神的讲解总结的，如果想看看更详细的讲解可以移步到[阮一峰es6详解](http://es6.ruanyifeng.com/#docs/promise)
## Promise含义
promise的意思是‘承诺’的意思，他表明在某个阶段改变自己状态，从而使后续代码执行
## 个人理解
我们经常看到这样子的写法
```javscript
let P = new Promise((resolve, reject) => {});
P.then(res => {})
```
我是这样理解的,首先分析这代码new Promise()，这样看，我们知道Promise就是一个内置构造函数，和Array,Object是一个道理，那么new Promise()这样一个操作，根据new的关键字我们知道，P肯定是一个实例化对象我们经常说Promise有三个状态pedding(进行中),resolve(已完成),reject(已失败),一旦pedding状态改变，then函数会执行或者catch函数会执行。那么我们就看看这句话let P = new Promise((resolve, reject) => {});那么这个P对象大概就是这个样子
P = {
  status: 'pedding',
  value: undefiend
}
然后:P.then()
P能够调用then函数，在这里只能说明一个道理，Promise.prototype.then,这个then是在Promise构造函数的原型上的。
## 个人猜想

P.then()函数他是一直监听P中的状态的，第一句，P的状态一直是pedding,then不会执行，所以他就等，等到
状态变成resolve,或者reject,所以一般我们这样写两秒之后，状态改变，then监听到了pedding变成了resolve,开始执行then里面的函数。
```javscript
let P = new Promise((resolve, reject) => {
  setTimeout(function() {resolve()}, 2000)
});
P.then(res => {})
//实质
P = {
  status: 'pedding',     
  value: undefiend
}
变成了
P = {
  status: 'resolve',     
  value: undefiend
}
```
大胆猜测resolve,干了什么事情
```javscript
resolve = () => {
 // 这里面是改变了Promise得状态得
  if ( this.status === 'pedding') {
    this.status = 'resolved';
}
  
}
```
但是着有个问题，resolve不管怎么调用，p实例得状态都会被改变，所以不能这么直接调用this,应该是一个_self状态
```javscript
const Promise = (fun) => {
_self = this;
resolve = () => {
 // 这里面是改变了Promise得状态得
  if ( _self .status === 'pedding') {
    _self .status = 'resolved';
}
  fun(resolve, reject)
}

}
```
## Promise特点
1：Promise的状态不会被外界改变得，只会被resolve,reject改变，然后我们一定要注意一个点，就是Promise得回调函数里面，第一个是resolve,第二个reject，很重要
2：Promise得状态一旦改变，就会停止，状态不会在被改变了。
## 简单使用
```javascript
const getData = () => {
  return new Promise((resolve, reject) => {
    resolve(55);
})
}
getData.then(res => {
  console.log(res) ; //55 
})
```

## Promise.all
```javascript
 const getData1 = () => {
  return new Promise((resolve, reject) => {
     setTimeout(() => {resolve(1)} , 2000)
})
}

 const getData2 = () => {
  return new Promise((resolve, reject) => {
     setTimeout(() => {resolve(2)} , 2000)
})
}

Promise.all([getData2(), getData1()]).then(res => {
    console.log(res); // [2, 1]
})
```
Promise.all接受一个数组。数组里面得每一个元素都是一个Promise实例，这个函数在数组里面所有得实例
得状态都改变(resolve)得时候，才会执行then函数,返回得结果是三个实例得resolve得累加，结果顺序和数组里面得顺序是一致得。如果其中有一个错误(reject()),就会但会这个错误值
## Promise.race
这个函数和Promise.all得写法一样，但是返回结果不一样，这个函数是如果所有得实例得状态都是resolve,他会但会最快那个resolve得结果,如果其中有一个是错得(reject()),它还是只会返回最快得那个，如果这个错得就是最快得那个，就会返回这个错得
## Promise.resolve()
这个直接Promise得静态函数，有四种状态相当于
Promise.resolve('aa')
==
new Promise((resove, reject) => resolve('aa'))
它就是返回 一个promise实例，但是状态是resolve
#### （1）参数是一个 Promise 实例
如果参数是 Promise 实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。
#### （2）参数是一个thenable对象
thenable对象指的是含有then函数得对象
```javascript
const obj = {
  then: function () {console.log(55)}
}
const p = Promise.resolve(obj);
// 如果resolve里面是一个thenable对象，则会马上执行一下这个then 函数
```
#### （3）参数不是thenable对象，或者根本不是一个对像或者为空，返回得是一个Promise,对象并且状态是resolved
```javascript
const p = Promise.resolve('aaa');
==>
const p = new Promise((resolve,reject) => resolve('aaa'));
```
## Promise.reject()
Promise。reject得函数得和Promise.resolve类似，但是有点不一样，
```javascript
 const p = Promise.reject('出错了');
==>
const p = new Promise((resolve, reject) => reject('出错了'))
```
但是不同点是，Promise.reject(参数)，参数不管是什么都会转给catch函数。包括thenable函数
```javascript
const obj = {
  then: () => {conosole.log(5455)}
}
const p = Promise.reject(obj);
p.catch((res) => res === obj) // ture
```
