---
title: less 小记
date: 2019-05-9 16:16:00
categories: 前端
tags:
    - css
    - less
---
## 介绍
less 是一种css预处理语言，赋予了css动态语言得特性，如变量，函数，使得css更容易维护和扩展。
## 用法特性
### 注释
注释有两种，一种是// 形式，编译后会被自动删除，第二个是/* */编译后会被保留
### 变量
less 可以允许你less文件中写变量，是的在文件其他地方使用
```less
@width: 500px;
@height: 500px;
.demo {
  width: @width;
  height: @height;
 background: color
}
```
上面得例子中，变量@width设置为500px;在.demo里面被使用
```less
@color: color

.@{color} {
  color: green
}
.demo {
  width: @width;
  height: @height;
  background-@{color}: red
}
```
变量还可以作为属性和差值来使用,上面得@color就是充当了属性和差值

### 混合
混合得意思是，less支持在一个class里面，混合写入其他class样式
```less
@width: 500px;
@height: 500px;
.box {
   width: @width;
  height: @height;
}
.border {
  border: solid 1px red
}
.demo {
 .box;
.border;
 background: color
}
```
.demo里面就可以使用。.box和.border内容了

### 混合--参数模式
混合还可以传入参数，和设置传入参数得默认值
```less
@width: 500px;
@height: 500px;
.box {
   width: @width;
  height: @height;
}
.border(@width:10px) {
  border: solid @width red
}
.demo {
 .box;
.border(20px);
 background: color
}
```
.border现在是一个带参数得class,其中10px是默认值。在.demo中使用得时候，一定要加上括号

### 匹配模式
我们写样式得时候，经常会要设置左border, 右border, 下border，为了防止重复得编写
```less
.border(left, @width: 30px) {
    border-left: solid @width black;
}

.border(right, @width: 30px) {
    border-right: solid @width black;
}

.border(bottom, @width: 30px) {
    border-bottom: solid @width black;
}

.test {
    .border(left, 10px);
    background-color: red;;
}

.demo {
    .border(bottom);
    background-color: green;
}
```

### 运算
less 也可以支持运算+, -, * ，/
```less
@length: 500px;

.demo {
  width: @length;
 height: @length / 2
}

```
这里得2可以是纯数字，也可以是2px,建议是用纯数字
### 命名空间
命名空间就是把公用得样式打包在一起，更好得管理css
```less
#btn {
    .primary {
        background-color: blue;
    }
    .error {
        background-color: red;
    }

    .default {
        background-color: gray;
    }
}
// 样式
.btn {
    width: 30px;
    height: 15px;
}

.btn-primary {
    .btn;
    #btn > .error
}
```
### 作用域
作用域和js得作用域也很相似，就是首先在内部找变量，没有找到，再去外面找，找到了就用内部得
```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}

#footer {
  color: @var; // red  
}
```
### @arguments, !important
@arguments表示混合时候得所有参数，在混合class后面加上important，那么里面所有得样式都会加上important
```less
@length: 500px;
.box {
    width: @length;
    height: @length / 2;
}
.border_all(@type:solid, @width:5px, @color: black) {
    border: @arguments;
}
.test {
    .box !important;
    .border_all();
    background-color: red;;
}

```
@arguments相当于@type @width @color，.box !important, 那么box里面的样式全部会加上imporant

### @import
#### reference
这个标记是说，导入得less之引用，并不输出到编译后得css中去
```less
// .btn.less
#btn {
    .primary {
        background-color: blue;
    }
    .error {
        background-color: red;
    }

    .default {
        background-color: gray;
    }
}

@import (reference) './btn.less';
.btn {
    width: 40px;
    height: 15px;
    display: inline-block;
}

.btn-primary {
    .btn;
    #btn > .primary
}
```
查看编译得结果并没有把btm.less输出，但是.btn-primary正常引用

#### inline
引入外部文件，但是不加工他们
```less
// .btn.less
#btn {
    .primary {
        background-color: blue;
    }
    .error {
        background-color: red;
    }

    .default {
        background-color: gray;
    }
}

@import (inline) './btn.less';
.btn {
    width: 40px;
    height: 15px;
    display: inline-block;
}

.btn-primary {
    .btn;
    #btn > .primary
}
```
查看结果:
![image.png](https://upload-images.jianshu.io/upload_images/13805935-0825f9ffe025f257.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### css 把引入得文件看成是css文件
```less
// .btn.less
#btn {
    .primary {
        background-color: blue;
    }
    .error {
        background-color: red;
    }

    .default {
        background-color: gray;
    }
}

@import (css) './btn.less';
.btn {
    width: 40px;
    height: 15px;
    display: inline-block;
}

.btn-primary {
    .btn;
    #btn > .primary
}
```
查看结果:
![image.png](https://upload-images.jianshu.io/upload_images/13805935-3d25bf54ed0ad4a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### less, once, multiple
less: 把引入得文件看成是less文件
once: 只引入一次
multiple: 可以引入多次

[http://www.bootcss.com/p/lesscss/#docs](http://www.bootcss.com/p/lesscss/#docs)
[https://www.html.cn/doc/less/features/#import-directives-feature](https://www.html.cn/doc/less/features/#import-directives-feature)

