---
title: es5和es6继承之我见
date: 2019-05-17 16:31:39
categories: 前端
tags:
    - js
    - 继承
---
### 前记
既然这里讲到的是继承，就不讲原型发面的知识了(就当你知道了)

### 原型继承
```javascript
const Parent = function (name, age) {
    this.name = name;
    this.age = age;
}

Parent.prototype.body = [1, 2, 3];
const Son = function (sex) {
    this.sex = sex;
} 

Son.prototype = Parent.prototype;
Son.prototype.constructor = Son;
const son = new Son('json', 24, 'nan');
const son1 = new Son('json', 24, 'nan');

console.log(son);
console.log(son.name);
console.log(son.age);
console.log(son.sex);
console.log(son.body);
console.log(son.height);
console.log(son.constructor);
```
![image](https://user-images.githubusercontent.com/20452750/57910825-bf481a80-78b8-11e9-8cb1-e23b4f85768c.png)
原型继承，就是Son.prototype = Parent.prototype;这是最简单的原型继承
#### 优点
-  父亲原型上的方法和属性在字类全部可以拿到
- 实现简单
#### 缺点
- 会带来原型引用的问题，一个实例改变，另一个也会改变
- 不会继承实例化属性
- 无法传参

#### 原型继承（优化）
```javascript
const Parent = function (name, age) {
    this.name = name;
    this.age = age;
    this.height = 170;
}
const Son = function (sex) {
    this.sex = sex;
} 

Son.prototype = new Parent();
Son.prototype.constructor = Son;
const son = new Son('json', 24, 'nan');

console.log(son);
console.log(son.name);
console.log(son.age);
console.log(son.sex);
console.log(son.body);
console.log(son.height);
console.log(son.constructor);
```
![image](https://user-images.githubusercontent.com/20452750/57911287-f539ce80-78b9-11e9-9c62-e5def1d3d968.png)
#### 总结
这次优化就是Parent.prototype;　换成new Parent()
#### 优点
-  父亲原型上的方法和属性在字类全部可以拿到
- 实现简单
- 可以继承实例化属性
#### 缺点
- 会带来原型引用的问题，一个实例改变，另一个也会改变
- 无法传参

### 构造函数继承
```javascript
const Parent = function (name, age) {
    this.name = name;
    this.age = age;
    this.body = [1, 2, 3];
}

// Parent.prototype.body = [1, 2, 3];
const Son = function (name, age, sex) {
    Parent.call(this, name, age);
    this.sex = sex;
} 

Son.prototype.constructor = Son;
const son = new Son('json', 24, 'nan');
const son1 = new Son('json', 24, 'nan');

console.log(son);
console.log(son.name);
console.log(son.age);
console.log(son.sex);
console.log(son.body);
console.log(son.constructor);
```
![image](https://user-images.githubusercontent.com/20452750/57911498-85781380-78ba-11e9-8980-e58e266e981b.png)
#### 总结
构造函数继承，其实就是在Son函数里面调用的了Paren.call(),然后把this的指向改变
#### 优点
- 可以实现实例化属性的继承
- 解决了原型链共享的问题
- 可以传参

#### 缺点
- 无法继承父类原型上的属性
- 函数无法被复用
- 实例只是Son的实例，不是Parent的实例
### 组合继承
```javascript
const Parent = function (name, age) {
    this.name = name;
    this.age = age;
}

Parent.prototype.body = [1, 2, 3];
const Son = function (name, age, sex) {
    Parent.call(this, name, age);
    this.sex = sex;
} 

Son.prototype = new Parent();
Son.prototype.constructor = Son;
const son = new Son('json', 24, 'nan');
const son1 = new Son('json', 24, 'nan');

console.log(son);
console.log(son.name);
console.log(son.age);
console.log(son.sex);
console.log(son.body);
console.log(son.constructor);
```
![image](https://user-images.githubusercontent.com/20452750/57911783-4bf3d800-78bb-11e9-8e97-a875f1c38bff.png)

#### 总结
组合继承就是结合了原型继承和构造器继承
#### 优点
- 继承父类的原型和实例化属性
- 函数可以复用
- 可以传参
#### 缺点
- 父构造器被调用了两次

#### 组合继承（优化）
```javascript
const Parent = function (name, age) {
    this.name = name;
    this.age = age;
}

Parent.prototype.body = [1, 2, 3];
const Son = function (name, age, sex) {
    Parent.call(this, name, age);
    this.sex = sex;
} 

// 第一种
Son.prototype = Parent.prototype;
// 第二种
Son.prototype = Object.create(Parent.prototype);
Son.prototype.constructor = Son;
const son = new Son('json', 24, 'nan');
const son1 = new Son('json', 24, 'nan');

console.log(son);
console.log(son.name);
console.log(son.age);
console.log(son.sex);
console.log(son.body);
console.log(son instanceof Son, son instanceof Parent);
console.log(son.constructor);
```
![image](https://user-images.githubusercontent.com/20452750/57912091-097ecb00-78bc-11e9-995a-cca231e6d0e4.png)
#### 总结
这两种优化，都是可以使得执行的构造函数次数为1次

### es6继承
```javascript
class Parent {
    constructor(name, age) {
       
        this.name = name;
        this.age = age;
    }

    state = {
        name: 'json'
    }
   
    getAge() {
        console.log(111)
    }
};

class Son extends Parent{
    constructor(name, age, sex) {
        super(name, age);
        this.sex =sex;
    }

    getSex() {
        console.log('00000')
    }
}

const son = new Son('json', 24, 'nan');
console.log(son);
```
![image](https://user-images.githubusercontent.com/20452750/57912246-6e3a2580-78bc-11e9-8322-5ee6b923d548.png)

####　总结
我们发现es6种class继承其中就是es5种的组合继承的优化

### 区别
es5 的继承和es6的继承主要是在this的创造上面
> es5继承的this是在子类先创造this,然后继承父类的属性和方法

> es6继承的this是先创造了父类的this,子类去修改这个this