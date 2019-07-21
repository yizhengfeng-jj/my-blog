---
title: webpack配置babel篇
date: 2019-04-26 16:13:47
categories: 前端
tags:
    - webpack
    - js
---
## 插件
### babel-loader
我们经常用bable来处理js的，这次记录一下网上的知识点，方便下次查看， 对于之前的babel来说，babel里面集合了很多的插件，是一个大杂烩，不需要安装过多的其他插件，但是babel 6以上就就已经把babel 和插件分开了，这样的好处，不需要的插件可以不需要使用，已经babel的扩展性提高
### babel-cil
bable-cil 是babel的命令行插件，意思就是说你可以在命令行工具里面使用babel 命令如
```javascript
babel app.js
```
### babel-core
babel-core和babel现在是形影不离的，babel-core的作用是以编程的方式来使用babel, babel-core把比较高级的js写法编译成AST语法树，然后利用其他插件，继续把语法树，编译成对应版本的js代码
```javascript
npm install babel-core
```
### babel-ployfill
babel-core 可以把es6等高级语法，比如箭头函数，const等转换成AST语法树，但是对于某些API（generator, Promise）他不能编译，所以这个时候需要用到垫片，babel-ployfill就是其中一个，他的作用是，babelc-core无法编译的API，重新用低版本js写一遍，这个插件，会把新编译好的语法，放在全局上，这样会造成了全局的污染，所以一半用在应用开发上。
```javascript
  npm install babel-polyfill
  import 'babel-polyfill' // 直接js引用就可以用了
```
### babel-plugin-transform
与 babel-polyfill 一样，babel-runtime 的作用也是模拟 ES2015 环境。只不过，babel-polyfill 是针对全局环境的，引入它，我们的浏览器就好像具备了规范里定义的完整的特性 – 虽然原生并未实现。
babel-runtime 更像是分散的 polyfill 模块，我们可以在自己的模块里单独引入，比如 require(‘babel-runtime/core-js/promise’) ，它们不会在全局环境添加未实现的方法，只是，这样手动引用每个 polyfill 会非常低效。我们借助 Runtime transform 插件来自动化处理这一切。他更适合框架型开发，比如VUE
```javascript
npm install babel-plugin-transform-runtime
npm install babel-runtime  // 这个也要安装，因为install babel-plugin-transform-runtime依赖他
```
### babel-preset-env
这个是preset类型插件，意思就是指定babel编译的环境，如果在此环境下支持该语法，就不需要去处理了
```javascript
{
  presets: [
  ['env', {
  targets.browsers: {"chrome" : 52}
}]
]
}
```
### babel-preset-es2015, babel-preset-2016, babel-preset-2017
这些presets系列的插件，就是去处理babel-core编译好的AST语法树，es2015打包es6语法，依次类推

## 配置
```javascript
  module.exports = {
    entry: '',
    output: '',
    rules: [
      {
          test: /\.(js)$/,
          loader: 'babel-loader',
          exclude: /node_modules/,
          options: {
            presets: ['es2015' , ['env', {
                targets: {
                  browsers: {
                      chrome: 52
                   }
                }
             }]],
            plugins: [
              'transform-runtime'
            ]
      }
      }
     ]
}
// babel-polyfill 可以不用写在这里面，因为他是直接在js里面引入的
```
最后，我们一半习惯把babel-loader 的配置写在.babelrc里面的


