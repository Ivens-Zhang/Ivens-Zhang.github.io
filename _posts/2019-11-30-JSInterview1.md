---
layout: post
title: "JavaScript 基础面试题——基础上"
subtitle: ""
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-11-30/th.jpg"
hidden: true
catalog: true
tags:
  - 前端学习笔记
---

- [前言](#%e5%89%8d%e8%a8%80)
- [正文](#%e6%ad%a3%e6%96%87)
  - [重点知识点](#%e9%87%8d%e7%82%b9%e7%9f%a5%e8%af%86%e7%82%b9)
  - [变量类型和计算](#%e5%8f%98%e9%87%8f%e7%b1%bb%e5%9e%8b%e5%92%8c%e8%ae%a1%e7%ae%97)
    - [知识点](#%e7%9f%a5%e8%af%86%e7%82%b9)
    - [JS 中使用 typeof 能得到哪些类型?](#js-%e4%b8%ad%e4%bd%bf%e7%94%a8-typeof-%e8%83%bd%e5%be%97%e5%88%b0%e5%93%aa%e4%ba%9b%e7%b1%bb%e5%9e%8b)
    - [何时使用 === 何时使用 ==](#%e4%bd%95%e6%97%b6%e4%bd%bf%e7%94%a8--%e4%bd%95%e6%97%b6%e4%bd%bf%e7%94%a8)
    - [JS 中有哪些内置函数?](#js-%e4%b8%ad%e6%9c%89%e5%93%aa%e4%ba%9b%e5%86%85%e7%bd%ae%e5%87%bd%e6%95%b0)
    - [JS 变量按照存储方式区分为哪些类型, 并描述特点](#js-%e5%8f%98%e9%87%8f%e6%8c%89%e7%85%a7%e5%ad%98%e5%82%a8%e6%96%b9%e5%bc%8f%e5%8c%ba%e5%88%86%e4%b8%ba%e5%93%aa%e4%ba%9b%e7%b1%bb%e5%9e%8b-%e5%b9%b6%e6%8f%8f%e8%bf%b0%e7%89%b9%e7%82%b9)
    - [如何理解 JSON ?](#%e5%a6%82%e4%bd%95%e7%90%86%e8%a7%a3-json)
  - [原型和原型链](#%e5%8e%9f%e5%9e%8b%e5%92%8c%e5%8e%9f%e5%9e%8b%e9%93%be)
    - [知识点](#%e7%9f%a5%e8%af%86%e7%82%b9-1)
    - [如何准确判断一个变量是不是数组类型?](#%e5%a6%82%e4%bd%95%e5%87%86%e7%a1%ae%e5%88%a4%e6%96%ad%e4%b8%80%e4%b8%aa%e5%8f%98%e9%87%8f%e6%98%af%e4%b8%8d%e6%98%af%e6%95%b0%e7%bb%84%e7%b1%bb%e5%9e%8b)
    - [写出一个原型链继承的例子](#%e5%86%99%e5%87%ba%e4%b8%80%e4%b8%aa%e5%8e%9f%e5%9e%8b%e9%93%be%e7%bb%a7%e6%89%bf%e7%9a%84%e4%be%8b%e5%ad%90)
    - [描述 new 一个对象的过程](#%e6%8f%8f%e8%bf%b0-new-%e4%b8%80%e4%b8%aa%e5%af%b9%e8%b1%a1%e7%9a%84%e8%bf%87%e7%a8%8b)
  - [作用域和闭包](#%e4%bd%9c%e7%94%a8%e5%9f%9f%e5%92%8c%e9%97%ad%e5%8c%85)
    - [知识点](#%e7%9f%a5%e8%af%86%e7%82%b9-2)
    - [说一下变量提升的理解](#%e8%af%b4%e4%b8%80%e4%b8%8b%e5%8f%98%e9%87%8f%e6%8f%90%e5%8d%87%e7%9a%84%e7%90%86%e8%a7%a3)
    - [说明this几种不同的使用场景](#%e8%af%b4%e6%98%8ethis%e5%87%a0%e7%a7%8d%e4%b8%8d%e5%90%8c%e7%9a%84%e4%bd%bf%e7%94%a8%e5%9c%ba%e6%99%af)
    - [创建10个`<a>`标签, 点击的时候弹出来对应的序号](#%e5%88%9b%e5%bb%ba10%e4%b8%aaa%e6%a0%87%e7%ad%be-%e7%82%b9%e5%87%bb%e7%9a%84%e6%97%b6%e5%80%99%e5%bc%b9%e5%87%ba%e6%9d%a5%e5%af%b9%e5%ba%94%e7%9a%84%e5%ba%8f%e5%8f%b7)
    - [如何理解作用域](#%e5%a6%82%e4%bd%95%e7%90%86%e8%a7%a3%e4%bd%9c%e7%94%a8%e5%9f%9f)
    - [实际开发中闭包的应用](#%e5%ae%9e%e9%99%85%e5%bc%80%e5%8f%91%e4%b8%ad%e9%97%ad%e5%8c%85%e7%9a%84%e5%ba%94%e7%94%a8)

## 前言

听从爸爸的劝说,昨晚试着在实习僧上找找附近有没有公司提供实习的机会.

结果发现两家觉得很不错,一家是趣头条,另一家是哔哩哔哩.

趣头条这家公司我只是很早之前听说过, 说实话这名字取得真不太好, 起得跟个垃圾软件一样. 不过后来查了下, 17 年左右已经在纳斯达克上市了, 应该也不会差的.

在看准网上查了下之前员工的一个评价, `人员流动性大` 、 `公司内部斗争严重` 、`加班多` 看来前员工对这家公司的评价倒是不太好.

最让我觉得有意思的是知乎上的一个评论, 说趣头条做聚合类新闻, 说自己是第二, 想想也没有问题. 第一是今日头条, 第二是趣头条, 第三是一点资讯. 可是这个类别里老二 + 老三 的份额乘以四都赶不上老大.

份额大小其实对一个实习生而言无差, 如果有机会去这种大公司里实习, 相信一定也能学到很多东西.

第二家公司则是哔哩哔哩, 这个不多说, 再熟悉不过. 昨晚花了一个多小时删改之前的简历, 晚上 11 点把简历发了过去, 现在正在等待消息.

这篇文章《 JavaScript 基础面试题(一) 》 也是写于这个情况之下, 可能收到面试的通知, 所以我需要提前做点准备, 毕竟在 JavaScript 这一块理解不够深入, 一些简单的逻辑都可以实现, 但是如果问些原理, 那就是一问三不知了.

本来现在还在按照计划学习 `Vue` 框架, 正准备开始做去哪儿仿站. 现在我不得不改变一下计划, 把原本准备放在最后的基础知识拿到现在来看.

现在做的这些努力也只是为了弥补大学前两年的荒废, 用一句话安慰自己 "尽人事,听天命".

## 正文

### 重点知识点

基础知识:

- 原型 原型链
- 作用域 闭包
- 异步 单线程

JS API:

- DOM 操作
- AJAX
- 时间不定

开发环境:

- 版本管理
- 模块化
- 打包工具

运行环境:

- 页面渲染
- 性能优化

### 变量类型和计算

#### 知识点

- 变量类型
  - 值类型与引用类型
  - typeof 运算符详解 
- 变量运算(值类型)
  - 字符串拼接
  - == 运算符

```js
100 == '100'      //true
0 == ''           //true
null== undefined  //true
// 慎用 == , 尽可能使用 ===
```
  - if 语句
  - 逻辑运算

#### JS 中使用 typeof 能得到哪些类型?
```js
typeof undefined    //undefined
typeof 'abc'        //string
typeof 123          //number
typeof true         //boolean
typeof console.log  //function
typeof {}           //object
typeof []           //object
typeof null         //object
// 1 2 3 4 行为值类型, 5 6 7 8 为引用类型
```
#### 何时使用 === 何时使用 ==
```js
if (obj.a == null) {
  // 这里的 'obj.a == null' 相当于 'obj.a === null || obj.a === undefined', 简写形式, 这是 JQuery 源码中的推荐写法.
}
```
除了以上的情况外, 其他均使用 `===`
#### JS 中有哪些内置函数?
```js
Object
Array
Boolean
Number
String
Function
Date
RegExp //正则表达式
Error
```

#### JS 变量按照存储方式区分为哪些类型, 并描述特点
分为: `值类型` 与 `引用类型` 

#### 如何理解 JSON ?
本质上是一个对象, 也是一种数据格式.

### 原型和原型链
#### 知识点
- 构造函数
- 构造函数拓展
  - `var a = {}` 等于 `var a = new Object()`
  - `function Foo(){}` 等于 `var a = new Function('...')`
- 原型规则和示例
  - 除 `null` 外,所有的引用类型( `数组` / `对象` / `函数` ), 都具有对象特性, 即可以自由拓展属性 
  - 所有的引用类型( `数组` / `对象` / `函数` ), 都有一个 `__proto__(隐式原型)` 属性, 属性值是一个普通的对象
  - 所有函数都有 `prototype(显式原型)` 属性, 属性值是一个普通对象
  - 所有的引用类型( `数组` / `对象` / `函数` ), `__proto__` 属性值指向它的构造函数的 `prototype` 属性值
  - 如果对象本身没有该属性, 那么会去它的 `__proto__` 中寻找.
- 原型链

![](../../../../img/in-post/2019-11-30/a.png)
- instanceof
  - 用于判断`引用类型`属于哪个`构造函数`的方法
  - 通过对象的 `__proto__` 一层层往上, 看能否找到对应的构造函数的 `prototype`
#### 如何准确判断一个变量是不是数组类型?
```js
var arr = [];
arr instanceof Array    //true
typeof arr              //object   typeof 是无法判断变量是否为数组的
```
#### 写出一个原型链继承的例子
```js
// 动物
function Animal() {
  this.eat = function () {
    console.log('animal eat');
  }
}

// 狗
function Dog() {
  this.bark = function () {
    console.log('dog bark');
  }
}

Dog.prototype = new Animal();

// 哈士奇
var haShiQi = new Dog();
```
#### 描述 new 一个对象的过程
1. 创建一个新对象
2. `this` 指向这个新对象
3. 执行代码, 即对 `this` 赋值
4. 返回 `this`

```js
function Foo(name, age) {
  this.name = name;
  this.age = age;
  this.class = 'class-1';
  // return this 这一行这是默认有的

  var 发= new Foo('Tom', 20);
  // 在实例化的时候先生成一个空对象,然后根据传入的参数对属性赋值, 再通过 return this 返回出来.
}
```

### 作用域和闭包
JavaScript 是解释型语言, 所以有些错误只有在执行时才会报错.
#### 知识点
- 执行上下文
  - 范围: 一段 `<script>` 或一个函数之内都会生成上下文
  - 全局: 变量定义 / 函数声明 / 一段 `<script>`
  - 函数: 变量定义 / 函数声明 / `this` / `arguments` (函数执行前会生成函数上下文)
  - 注意: `函数声明` 和 `函数表达式` 的区别 
- `this`
  - 只有在执行时才能确认值,定义时无法确认

```js
var a = {
  name: "A",
  fn: function() {
    console.log(this.name);
  }
};
a.fn();     //输出为 A, 此时 this 为 a
```
```js
var a = {
  name: "A",
  fn: function() {
    console.log(this.name);
  }
};
a.fn.call({name:'B'});    //输出为: B, 此时 this 为 {name:'B'} 
```
```js
var a = {
  name: "A",
  fn: function() {
    console.log(this.name);
  }
};

var fn1 = a.fn;
fn1();      //输出为: undefined, 此时 this 为 window
```
- 作用域
- 作用域链
- 闭包

#### 说一下变量提升的理解
#### 说明this几种不同的使用场景


#### 创建10个`<a>`标签, 点击的时候弹出来对应的序号
#### 如何理解作用域
#### 实际开发中闭包的应用