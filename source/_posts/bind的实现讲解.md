---
title: bind的实现讲解
date: 2019-07-05 16:33:53
tags:
    - js
    - 手写代码
---
### 前言
实现一个bind的方法，让自己对代码的动手能力更强

### 分析
我们写一个bind的功能，要满足的条件有哪些
- 1： 我们写的函数要定义在Function的原型上
- 2：bind能够让我们改变this的执行
- 3：bind不是立即执行，而是返回一个函数
- 4：如果bind之后，函数是new执行的，那么就this绑定将失效

### 代码解析
```
   Function.prototype.bindCopy = function () {
      
    return function () {
        
     }
   }
```

#### 分析1
上面实现了在Function定义一个函数，并且该函数返回了一个函数，那么现在我们把参数的传递写进去，因为bind的参数是可以传递的，所以代码如下
```javascript
  Function.prototype.bindCopy = function (...arguments) {
      // 获取this要绑定的对象, 就是第一个参数
    const bindObj = Array.prototype.slice.call(arguments, 0, 1)[0];
   
     // 剩下的参数就是传递给函数的参数
    const paramsToFn = Array.prototype.slice.call(arguments, 1);
     
    // 获取调用bind的函数
   const executeFn = this;
 
    return function (...args) {
        // 全部参数
       return  executeFn .call(bindObj, ...paramsToFn , ...args);
     }
   }
```
#### 分析２
这里为什么要return了？就是防止调用bind的函数，就是有return操作的
```javascript
const Parent = function (name, age) {
  this.name = name;
  this.age = age;
}
const obj = {};
const parent = Parent.bind(obj, 'json');
//parent(25)；
//new parent(25)
```
#### 分析3
我们上面的代码已经可以做到如下的调用,只是对于new 的操作没有一个很好的支持，调用new的话，还是一样的结果, this都是指向了obj,怎么判断是new操作，并且让在new操作时候，指向正确是关键
```javascript
  Function.prototype.bindCopy = function (...arguments) {
      // 获取this要绑定的对象, 就是第一个参数
    const bindObj = Array.prototype.slice.call(arguments, 0, 1)[0];
   
     // 剩下的参数就是传递给函数的参数
    const paramsToFn = Array.prototype.slice.call(arguments, 1);
     
    // 获取调用bind的函数
   const executeFn = this;
   
    const callbackFn =  function (...args) {
        // 全部参数
       const ifExecuteNew = this instanceof executeFn ;
       return  executeFn .call(ifExecuteNew ? this : bindObj, ...paramsToFn , ...args);
     }

   callbackFn .prototype = this.prototype;
   
   return  callbackFn;
   }
```
#### 分析4
new 完之后，返回的对象的__proto__确实指向了Parent函数的prototype,但是callbackFn .prototype = this.prototype这样写的弊端是callbackFn 的原型改了把Parent的原型也改了，所以比较经典的做法是创造一个中间函数
```javascript
 Function.prototype.bindCopy = function (...arguments) {
      // 获取this要绑定的对象, 就是第一个参数
    const bindObj = Array.prototype.slice.call(arguments, 0, 1)[0];
   
     // 剩下的参数就是传递给函数的参数
    const paramsToFn = Array.prototype.slice.call(arguments, 1);
     
    // 获取调用bind的函数
   const executeFn = this;
   
    const callbackFn =  function (...args) {
        // 全部参数
       const ifExecuteNew = this instanceof executeFn ;
       return  executeFn .call(ifExecuteNew ? this : bindObj, ...paramsToFn , ...args);
     }
   
   const middleFn = function() {}; 
   middleFn .prototype = this.prototype;
  callbackFn.prototype = new middleFn()
   
   return  callbackFn;
   }
```
