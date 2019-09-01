---
title: antd form表单自定义组件
date: 2019-09-01 18:06
categories: 前端
tags: 
   - antd
---

### 前言
之前我们我们用antd表单的时候，总是想着设计能够用antd上已经有的控件，这样我们才可以用getFieldsValue获取对应的值，如果表单里面的的组件是一个不常见的组件，我们最初想法是不能设置initialValue的，或者说设置了没有效果，但是这样的设计可能太单一了，于是表单里面就有很多复杂的需要自己自定义的组件，那么这种情况就没有办法做了么？接下来就来说一下如何处理这种问题。

### 要满足的场景
#### 场景1：getFieldsValue() 可以获取到值
这个函数是获取的组件要设置的值，那么这个值怎么设置了，经过研究发现，当一个组件被getFieldsDecorator包裹的时候，会附带一些信息，其中有一个函数名字叫onChange，只要把要返回的值传递给这个函数执行一遍，那么getFieldsValue就可以获取到值了，我们看看打印的信息
![image](https://user-images.githubusercontent.com/20452750/64074284-dc73ee00-ccdb-11e9-95ee-435d98641eb2.png)
点击changeType打印了一下this.props发现有一个onChange函数,那么我们执行一下这个函数，设置固定值'小明',然后点击获取值按钮看看效果

![image](https://user-images.githubusercontent.com/20452750/64074192-dc272300-ccda-11e9-9108-47f44694111b.png)
点击自定义组件的changeType函数，把想要传入的值写入this.props.onChange调用就可以了，那么就可以用getFieldsValue这个函数去获取了

#### 场景2：initialValue有效果么
这个场景经常出现在新建，编辑的时候我们要获取初始值，绑定到自定义组件上去，那么这种情况我们怎么去获取了，还是一样的，初始值的信息也是在this.props里面，this.poprs里面有一个value，这个value就是初始值，里面还有两个data__mate和data__field两个对象也会保留这个initialValue的内容。我们还是打印看一下
![image.png](https://upload-images.jianshu.io/upload_images/13805935-4ce08caa81996d00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个this.props.value就是我们传入的initialValue的值，那么既然我们能拿到这个初始值，在自定义组件去设置就不难了，如上截图。

#### 场景3：setFieldsValue()，getFieldsError(), validateFields(),isFieldsTouched() 等函数会有效果么
这写函数经过试验都是有效果的，setFieldsValue这个函数执行完之后，this.props.value自动会改变，并且会重新渲染，那么页面显示肯定就是你设置的那个，getFieldsError这个就是获取的报错，经验证是可以获取自定义组件报错信息的，validateFields这个执行完之后，他会去检查你的设置值，如果是'',null,udefiend就会报错，经验证也是可以的，isFieldsTouched这个函数是如果你没有点击changeType函数返回fasle,如果点击了返回true，感觉就是检测你有没有触发this.props.onChange函数

#### 场景4：自定义校验满足么
自定义校验一个很重要的点，就是你要知道你现在组件设置的值，然后根据这个值去做判断，那么会不会返回这个设置值就是关键了,看如下截图
![image.png](https://upload-images.jianshu.io/upload_images/13805935-b135000d9b0524e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
下面这个图告诉我们当我们自定义检测的时候，我们是可以拿到设置的值得，从而去设置错误信息

### 注意事项
- 因为自定义组件被getFieldDecorator包装后会自带onChange函数，所以自定义组件的函数尽量不要再有onChange，换一个其他名字
- 因为initialValue的值是写在this.props.value里面的，所以自定义组件也不要在传入value属性了，会报一个警告
![image.png](https://upload-images.jianshu.io/upload_images/13805935-06c41da6696b52ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 调用this.props.onChange或者setFieldsValue组件会被重新渲染一次（很重要）

