---
layout: post
title: "面向 2020 春招面试题(三)"
subtitle: "————JavaScript 部分考点"
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-12-4/th.jpg"
hidden: true
catalog: true
tags:
  - 春招面试题
---

## 一. 数据类型

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200105120329.png)

### 1. 在内存中位置的不同
- **基本类型**：占用 `空间固定`，保存在 `栈中`；
- **引用类型**：占用 `空间不固定`，保存在 `堆中`；

> `栈（stack）`为自动分配的内存空间，它由系统自动释放；使用一级缓存，被调用时通常处于存储空间中，调用后被立即释放。
> 
> `堆（heap）`则是动态分配的内存，大小不定也不会自动释放。使用二级缓存，生命周期与虚拟机的GC算法有关。

---

### 2. 赋值、浅拷贝、深拷贝

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200105154358.png)

---

### 3. `typeof` 运算符
`typeof` 将类型分为 : 原始数据类型, 复杂数据类型.

```js
typeof undefined    //undefined
typeof 'abc'        //string
typeof 123          //number
typeof true         //boolean
typeof console.log  //function
typeof {}           //object
typeof []           //object
typeof null         //object
```

当我们想清空数据时:
- 清空变量 : undefined
- 清空对象 : null

*提示 : 空值与 undefined 不是一回事, 空字符串变量`既有值也有类型`。*
```
var car = "";                // 值是 ""，类型是 "string"
```

---

### 4. `==` 运算符

**`==` 运算符只能比较值不能比较类型, 而 `===` 运算符可以连同类型一起比较.**

如 : `Undefined` 与 `null` 的值相等，但类型不相等.
```js
null == undefined  //true
null === undefined //false
```

所以, 在 JavaScript 中必须使用 `===`, 只有一种例外 :
```js
if (obj.a == null) {
  // 这里的 'obj.a == null' 相当于 'obj.a === null || obj.a === undefined'的简写形式.
  // 这是 JQuery 源码中的推荐写法.
}
```

## 二. 原型和原型链

### 1. 语法糖
语法糖是官方提供给我们的简便写法 :

| 语法糖写法|正常写法 |
|--|--|
| `var a = {}`|`var a = new Object()` |
| `var b = []`|`var b = new Array()` |
| `function C(){}`|`var C = new Function(){}` |

### 2. 什么是原型 ?

在 `JavaScript` 中，我们创建一个`函数A`, 浏览器会自动在内存中创建一个对象B，每个函数都默认会有一个属性 `prototype` 指向了这个自动创建的对象。

这个`对象B`就是`函数A`的**原型对象**，简称函数的原型。

这个原型对象B 默认会有一个属性 `constructor` 再指向函数A。

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200105181322.png)

**原型是 `Javascript` 中的继承的基础，继承就是基于原型的继承。**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200105232120.png)

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200105232124.png)

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200105231617.png)

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/a.png)

[*原型链详图*](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200105233426.png
)

instanceOf

### JavaScript 中继承是怎么实现的 ?

*请参考 :* [*《 JavaScript实现继承 》*](https://segmentfault.com/a/1190000016184431#item-5)

new 和 object.create 区别

在js中，创建对象有三种方式 ：
1. {}
2. new Object()
3. Object.create()

1和2的区别很小，1不能传参，2可以传参。

## 三. 作用域与作用域链

### 1. 变量提升
**JavaScript 中，变量可以在使用后声明，也就是变量可以先使用再声明。**

一般变量的声明为以下形式：
```js
var a=10;
```
js 在预编译阶段，是这么处理的：
```js
//预编译阶段
var a;
//执行阶段
a=10;
```
预编译阶段，js 将其分解为`变量声明`与`变量赋值`。

一般函数声明：
```js
function f(){
    console.log("declaration");
}
```
预编译阶段结果: 
```js
//预编译
var f;
f = function () {
  console.log("declaration");
}
```
---

**那么看两段相似的代码**
```js
f();//=>?
var f = function () {
  console.log("var");
}
 
function f() {
  console.log("function");
}
```
控制台打印结果 "function"。另一段代码:
```js
var f = function () {
  console.log("var");
}
 
function f() {
  console.log("function");
}
f();//=>?
```
控制台打印结果 "var"。

**为什么会有这样结果上的差异呢? 我们可以看一下**

因为在预编译阶段函数声明提前优先于函数表达式. 因此第二段代码在预编译时会做如下的处理:
```js
/* 预编译阶段
 *函数声明提前优先级别更高，先进行预编译,并对f进行了赋值。
 *在预编译阶段后于函数声明，f经历两次赋值，后来的赋值替代了原先的赋值，表现为f执行函数表达式。
 */
 
//函数声明预编译阶段
var f;
f = function () {
  console.log("function");
}
//函数表达式提前预编译,由于它是变量声明，变量声明提前是只有声明提前，而没有赋值提前。
//重复的声明，js会忽略
var f;
/*执行阶段*/
//变量f再次赋值
f = function () {
  console.log("var");
}
f();//=>"var"
```