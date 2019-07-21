---
title: html小谈
date: 2019-05-13 16:28:41
categories: 前端
tags:
    - html
---
### 常见的内联元素
- b, big, small, tt, i
- em, code, strong, abbr, acronym, cite, dfn, kbd, samp, var
- a, img, script, br, sub, sup, span,  bdo, object, q
- button, textarea, input, select, label

### 常见的块级元素
1：<address> 联系方式信息
2：<artical> 文章内容
3：<aside> 伴随内容
4： <audio> 音频播放
5：<bolckquote> 块级引用
6：<canvas> 图形
7：\<dl\> \<ol\> \<ul\>列表
8：<fieldsetm    >表单元素分组。
9：<figcaption> HTML5图文信息组标题
10：<figure> HTML5图文信息组 (参照 <figcaption>)。
11：<footer> 表示尾部
12：<form>表单
13：\<h1\>, \<h2\>, \<h3\>, \<h4\>, \<h5\>, \<h6\>标题级别 1-6.
14：<header> 表示头部
15：\<p\> 行
16：\<pre\> 预格式化文本
17：<selection> 页面区块
18：<video> 视频文件
19：<iframe> 内嵌框架

### 替换元素和非替换元素
替换元素和非替换元素是相对的，所以我们知道什么是替换元素，那么剩下的就是非替换元素，那么什么是非替换元素了，
定义：不受css视觉格式化模型控制，css渲染不考虑其内容，一般有固定的宽和高
也就是说是替换元素是浏览器根据元素的标签和属性来显示其具体的内容的

### 常见的行内替换元素
img, input, textarea, select, 这些元素可以看到都有自己的一套css列表，并且有的显示是更具自己的属性决定的，如img 的src ,input得type
![image](https://user-images.githubusercontent.com/20452750/56493124-a732e480-6520-11e9-936c-9efef056fdf6.png)

### 常见得块级替换元素
iframe, audio, video

### 行内元素， 块级元素，替换元素得区别
既然知道了行内元素和块级元素，为什么还要知道哪些是替换元素了，我们首先来看看区别
行内元素特点：不会独自成为一行，不能设置宽和高（行内替换元素除外），margin只有水平方向有效果,padding在垂直方向不会增加行高,但是会增加内容区域,也就是不会撑开副元素
原因：之所以不能设置padding，是因为padding的值是根据目标元素的width计算出来的，而inline， non-replace元素的width是不确定的
块级元素特点:会独自成为一行，可以设置高和框，padding和padding上下左右都有效果
其中比较特别得是行内替换元素
行内替换元素：不会独自成为一行，但是可以设置宽和高，padding和margin四个方向都有效果

## 常见html问题

### doctype得作用
doctype得作用是告诉浏览器用什么模式去解析html文档，模式分为两种一种是怪异模式，一种是标准模式，怪异模式下页面显示内容和dom得标准值不同，并且不同浏览器怪异模式还会不一样，所以最好告诉浏览器如果去用什么模式去解析html
#### html4
hrml4 有三种解析模式
> 严格模式 

\<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"  "http://www.w3.org/TR/html4/strict.dtd"\>
> 过度模式

\<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"  "http://www.w3.org/TR/html4/loose.dtd"\>

> 框架模式

\<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN"  "http://www.w3.org/TR/html4/frameset.dtd"\>

html5 只有一种写法
> 标准模式

HTML5不基于SGML，所以不需要引用DTD。在HTML5中<!DOCTYPE>只有一种
<！DOCTYPE html>

## SGML, XML, HTML得区别
上文提到SGML，但是这三者到底是什么关系了
SGML:标准通用标记语言，需要定义DTD来定义解析规则
XML: 可扩展得标记语言，用来传输数据得
HTML:HTML4及以前是SGML衍生过来得， HTML5不依赖SGML，所以不需要定义DTD
XHTML：XHTML是用XML规范来重新规范html，更严格一点

## html5新增了哪些东西
1：增加了语义化标签\<nav\>, \<footer\>, \<header\>,\</artical\>
2：表单增强type增加多种属性number,url,email等
3：标签增加了新属性 \<div data-friut="aa" id="demo"\>\</div>
获取得时候直接写demo.dataset.friut
4：废除了\<frame\>, \<center\>, \<big\>等可以用css实现得标签

## 语义化标签有什么用
1：模块化，便于团队开发
2：有利于爬虫看懂我们得网页
3：有利于SEO优化