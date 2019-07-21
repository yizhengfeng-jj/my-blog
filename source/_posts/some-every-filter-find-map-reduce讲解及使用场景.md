---
title: 'some, every, filter, find,map, reduce讲解及使用场景'
date: 2019-03-21 15:56:14
categories: 前端
tags:
    - js
---
### 前言
之前对数组的循环来说，我拿到代码就使用forEach循环，主要原因是我对数组的其他方法不是很了解，以及觉得forEach已经可以做到想要的效果，没必要在去使用其他方法，这都是对使用场景不了解导致的，为了让自己的代码不再是千篇一律的forEach。特地总结一下这几个方法。

### 函数1：some
函数形式：arr.some((value, index, arr) => {})
参数说明：第一个是每一个选项的值，第二个是索引，第三个是数组本身
使用场景：表示数组中是否有一项满足条件的，如果有就返回true,终止循环，如果没有，一直循环到结束，返回false，如果需要判断只要有一项满足就可以使用改方法
之前判断是否重名用的都是forEach这次可以换成some方式解决问题
```javascript
// 之前的forEach写法如下
var arr = [{name: 'aaa', id: 1}, {name: 'bbb', id: 2}, {name: 'ccc', id: 3}, {name: 'ddd', id: 4}];
var testName = 'aaa';
var ifCount = 0;
    var arrTest = arr.map(value => {

        if (value.name === testName) {
            ifCount++;
        }
    });

    if (ifCount !== 0) {
        console.log('重名了');
    }

// 下面用some方法来解决
let repeation = arr.some(value => {
  return value.name === testName;
})

if (repeation) { // 只要repeation为true说明找到了符合一模一样的名字，那么就表示重名了discover_save.js-108/new_template-333
 console.log('重名了');
} 
```
### 函数2：every
函数形式：arr.every((value, index, arr) => {})
参数说明：第一个是每一个选项的值，第二个是索引，第三个是数组本身
使用场景：表示数组中是否每一项满足条件的，如果是就返回true,如果有一项不满足则返回false,终止循环
重名判断用every来解决
```javascript
// 下面用every方法来解决
let repeation = arr.every(value => {
  return value.name !== testName;
})

if (!repeation) { // 只要repeation为false说明找到了符合一模一样的名字，那么就表示重名了discover_save.js-108
 console.log('重名了');
} 
```
### 函数3：filter
函数形式：arr.filter((value, index, arr) => {})
参数说明：第一个是每一个选项的值，第二个是索引，第三个是数组本身
使用场景：表示来获取符合条件的每一个选项，并返回。如果没找到就返回一个空数组,如果一个数组是根据另一个数组按照某一个条件得到的可以用该方法
```javascript
var arr = [{name: 'aaa', id: 1}, {name: 'bbb', id: 2}, {name: 'ccc', id: 3}, {name: 'ddd', id: 4}];
var test2 = arr.filter((value, item, arr) => { // /object/_object.js 11行
        return value.id > 1;
    }) 
```
### 函数4：find
函数形式：arr.find((value, index, arr) => {})
参数说明：第一个是每一个选项的值，第二个是索引，第三个是数组本身
使用场景：表示来查找符合条件的任意一个，找到就返回改项，没有就返回undefined, 此方法和some方法差不多，但是此方法可以把找到的选项信息打印出来.
```javascript
var arr = [{name: 'aaa', id: 1}, {name: 'bbb', id: 2}, {name: 'ccc', id: 3}, {name: 'ddd', id: 4}];
var test2 = arr.find((value, item, arr) => {
        return value.id > 1;
    })
```
### 函数5：map
函数形式：arr.map((value, index, arr) => {})
参数说明：第一个是每一个选项的值，第二个是索引，第三个是数组本身
使用场景：map就是用来对数组进行一个处理，返回一个新的数组，在需要返回一个新的数组中使用
```javascript
var arr = [{name: 'aaa', id: 1}, {name: 'bbb', id: 2}, {name: 'ccc', id: 3}, {name: 'ddd', id: 4}];
var test2 = arr.map((value, item, arr) => { // new_template 92
        return value.id > 1;
    })
```
### 函数6：reduce
函数形式：arr.reduce((pre,value, index, arr) => {}，initArr);
参数说明：
#### 情况1(没初始化值情况下)
第一次时候pre表示第一个选项，value表示第二个选项，index表示当前项的下标（此时当前项目是第二项），arr表示原数组
第二次时候pre表示的是return出来的值，value表示下一个选项，一次类推
#### 情况2（有初始化值的情况下）
第一次时候pre表示初始化的值，value表示第一个选项，index表示第一个选项索引，arr表示数组本身
第二次时候pre表示返回的值，value表示第二个值，依次类推

使用场景：需要累加数组值，计数的时候可使用
```javascript
// 累加arr的值
let arr = [1, 2, 3, 4];
let total = arr.reduce((pre, value, index, arr) => {
return pre + value;
}) ;
consoel.log(total);

// 计数值大于2的有几个
let arr = [1, 2, 3, 4];
let total = arr.reduce((pre, value, index, arr) => {
    
if (value > 2) {
  return pre + 1;
}
return pre;
}, 0);
console.log(toatl);
```


