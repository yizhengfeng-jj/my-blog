---
title: js设计模式之--代理模式
date: 2019-01-12 16:29:12
categories: 前端
tags:
    - js
    - 设计模式
---
### 定义
代理模式就是一个对象找另一个对象，对原对象进行访问
### 作用
1：在客户对象和目标对象之间做一个中介，降低客户对象和目标对象的耦合度
2：在调用目标对象之前可以做一些其他的操作
### 事例场景
小明追女生的故事，假如小明认识了一个女神，刚认识两天，但是已经对女神有好感了，于是想给女神送一束花，现在我们用代码把这段故事描述一下
```javascript
let Flower = function(){};
let xiaoMing = {
  sendFlower: function (target) {
   let flower = new Flower();
    target.receiveFlower(flower)
}
};
let nvShen = {
  receiveFlower: function (flower) {
  console.log('收到花了' + flower)
}
} 
xiaoMing.sendFlower(nvShen);
```
以上的代码就是这个故事的描述，但是小明和女生刚刚认识两天，就给女神表白，成功的机会就是百分之五十，如果女神心情好了，成功机会很大，如果女神心情不好了，成功机会就会很低，这个时候怎么办，那么就需要一个代理出现了，增加 表白的成功率，就故事而言这个代理就是女声的闺蜜，因为闺蜜和女神经常在一起，她知道女神的心情，知道女神什么时候心情好，用代码表示如下
```javascript
let Flower = function() {};
let xiaoMing = {
  sendFlower: function (target) {
   let flower = new Flower();
    target.receiveFlower(flower)
}
};
let daiLi = {
  receiveFlower: function(flower) { // 代理接收到花感知到了女神心情好，开始送花
    nvShen.goodMood(function () {
      nvShen.receiveFlower(flower);
  })
  }
};
let nvShen = {
  receiveFlower: function (flower) {
  console.log('收到花了' + flower)
},
goodMood: function (fn) {
  setTimeout(function() {
  fn() // 假如女神5秒后心情好
}, 5000)
}
} 
xiaoMing.sendFower(daiLi);
```
代码写道这里好像我们发现就故事情节而言是合理的，但是就代理角度而言不是很能理解啊，因为xiaoming同样也能调用nvShen.goodMood方法啊，在这里小明自己实行送花代码不行么，何必多此一举用个代理了。这个想法没有问题，不过如果这样的写的话，会导致代码冗余，为什么这么说假如不止小明追求女神，还有两个小王和小刚也在追求女神，那么代码就要像这样写了
```javascript
let xiaoMing = {
  sendFlower: function (target) {
   let flower = new Flower();
    taget.goodMood(function () {
  target.receiveFlower(flower)
})
}
};
let xiaoWang = {
  sendFlower: function (target) {
   let flower = new Flower();
    taget.goodMood(function () {
  target.receiveFlower(flower)
})
}
};
let xiaoGang = {
  sendFlower: function (target) {
   let flower = new Flower();
    taget.goodMood(function () {
  target.receiveFlower(flower)
})
}
};

//这样写代码就会比较重复了，所以我们用个代理这样写
let xiaoMing = {
  sendFlower: function (target) {
    target.receiveFlower(flower)
}
};
let xiaoMing = {
  sendFlower: function (target) {
    target.receiveFlower(flower)
}
};
let xiaoMing = {
  sendFlower: function (target) {
    target.receiveFlower(flower)
}
};

let daiLi = {
  receiveFlower: function(flower) { // 代理接收到花感知到了女神心情好，开始送花
    nvShen.goodMood(function () {
let flower = new Flower();
      nvShen.receiveFlower(flower);
  })
  }
};
xiaoMing.sendFlower(daiLi);
xiaoWang.sendFlower(daiLi);
xiaoGang.sendFlower(daiLi);
```
### 保护代理
以上的代理只是说明了代理的一点，从客户对象来精简了代码，在生活中，我们的明星经常有自己的经纪人，这些经济人就为这个明显提供保护，价格低的广告不接，不正当的场所不去，等等。这种保护目标对象的代理模式就是保护代理，就我们这个追女神的故事而言，我们现在让这个闺蜜代理成为一个保护代理，女神喜欢年龄在25对以下 的，大叔请绕道，那么我们把代理改一下如下
```javascript
let Flower = function() {};
let xiaoMing = {
  sendFlower: function (target, condition) {
    target.receiveFlower(condition)
}
};
let xiaoGang = {
  sendFlower: function (target, condition) {
    target.receiveFlower(condition)
}
};
let daiLi = {
  receiveFlower: function(condition) { // 代理接收到花感知到了女神心情好，开始送花
if ( condition.age > 25) {
  nvShen.reject();
  return;
}
    nvShen.goodMood(function () {
    let flower = new Flower();
      nvShen.receiveFlower(flower);
  })
  }
};
let nvShen = {
  receiveFlower: function (flower) {
  console.log('收到花了' + flower)
},
reject: function () {
console.log('不接受大叔')
},
goodMood: function (fn) {
  setTimeout(function() {
  fn() // 假如女神5秒后心情好
}, 5000)
}
} 
xiaoMing.sendFlower(daiLi, {age: 23});
xiaoGang.sendFlower(daiLi, {age: 32});
 ```
看到这里我们知道了，保护代理其实就是做了一个访问对象的筛选，我们这个时候或许还有一个疑问，我们可以把删选条件写道女神自己的对象里面把，如下
```javscript
let nvShen = {
  receiveFlower: function (flower, condition) {
   
if (condition.age > 25)  {
  console.log('我接受大叔')
} else {
   console.log('收到花了' + flower)
}
},
goodMood: function (fn) {
  setTimeout(function() {
  fn() // 假如女神5秒后心情好
}, 5000)
}
} 

// 把条件写在这里看似是没问题的，但是如果有多个女神都不喜欢大叔怎么半，三个女神还是会重复同样的代码，跟上面情况是一样的
let Flower = function() {};
let xiaoMing = {
  sendFlower: function (target, love,condition) {
    target.receiveFlower(love, condition)
}
};
let xiaoGang = {
  sendFlower: function (target, condition) {
    target.receiveFlower(condition)
}
};
let daiLi = {
  receiveFlower: function(love, condition) { // 代理接收到花感知到了女神心情好，开始送花
if ( condition.age > 25) {
  love.reject();
  return;
}
    love.goodMood(function () {
    let flower = new Flower();
      love.receiveFlower(flower);
  })
  }
};

let nvShen1 = {
  receiveFlower: function (flower) {
  console.log('收到花了' + flower)
},
reject: function () {
console.log('不接受大叔')
},
goodMood: function (fn) {
  setTimeout(function() {
  fn() // 假如女神5秒后心情好
}, 5000)
}
} 

let nvShen2 = {
  receiveFlower: function (flower) {
  console.log('收到花了' + flower)
},
reject: function () {
console.log('不接受大叔')
},
goodMood: function (fn) {
  setTimeout(function() {
  fn() // 假如女神5秒后心情好
}, 5000)
}
} 

xiaoMing.sendFlower(daiLi, nvShen1, {age: 21});
xiaoMing.sendFlower(daiLi, nvShen2, {age: 30});
```
这个代理模式还有一个就是符合开闭原则[开闭原则](https://segmentfault.com/a/1190000013123183);
一旦女神对男盆友没有任何要求，我们和女神关系很好，她也喜欢上了你，你不比在在意她的心情是好是坏，对于不用代理模式来说，你就得删除这个附加条件把，但是对于用了代理模式而言只需讲调用模式更改就行
```javascript
xiaoMing.sendFlower(nvShen); 
```