---
layout: post
title: "JavaScript 基础面试题——基础"
subtitle: ""
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-11-30/th.jpg"
hidden: true
catalog: true
tags:
  - 前端学习笔记
---

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
  - JavaScript 中没有块级作用域, 只有`函数`和`全局`作用域
- 作用域链

在当前作用域如果找不到使用的变量, 则会往上一级父作用域寻找, 如果找不到则会一直找到全局作用域.

**注意：** 函数的父级作用域是函数定义时候的作用域，不是函数执行时候的作用域. 也就是说哪个作用域定义了这个函数，这个函数的父级作用域就是谁，跟函数执行没有关系，函数自由变量要到父级作用域中找，就形成了作用域链
![](../../../../img/in-post/2019-11-30/b.png)
- 闭包
  - 闭包使用场景
    - 函数作为返回值
    - 函数作为参数传递

```js
// 1.函数作为返回值
function F1(){
  var a = 100;

  // 返回一个函数
  return function () {
    console.log(a);
  }
}
// 此时 f1 得到一个函数
var f1 = F1();
var a = 200;
f1();     //注意: 此时输出值为: 100
// 两个 a 完全不一样, 因为作用域不同. 是两个不相关的变量

// 2.函数作为参数传递
function F1(){
  var a = 100;

  // 返回一个函数
  return function () {
    console.log(a);
  }
}
var f1 = F1();
function F2(fn) {
  var a = 200;
  fn();
}
F2(f1);     //此时输出的还是 100, 因为定义时的作用域中 a = 100.
```
**函数的父级作用域是函数定义时候的作用域，不是函数执行时候的作用域.**

#### 说一下变量提升的理解
两种情况会出现变量提升:
1. 变量的定义
2. 函数的声明（注意和函数表达式的区别）
#### 说明this几种不同的使用场景
```js
// 作为构造函数执行
function Foo(name) {
  this.name = name;
}
var f = new Foo('zhangsan');

// 作为对象属性执行
var obj = {
  name: 'A',
  printName: function () {
    console.log(this.name);
  }
}
obj.printName();

// 作为普通函数执行
function fn() {
  console.log(this);
}
fn();     //此时 this === window


// call apply bind
function fncall(name,age) {
  alert(name + age);
  console.log(this)     //此时 this === window
}
fncall.call({x:100},'Tom',20);
// 此时弹出: Tom20, 输出: {x:100}

// apply 与 call 作用类似, 只不过 apply 的参数是以数组形式传进对象的.

var fnbind = function (name, age) {
  alert(name + age);
  console.log(this)
}.bind({y:200})
fnbind('John',15);
// 此时弹出: John15, 输出{y:200}
```
#### 创建10个`<a>`标签, 点击的时候弹出来对应的序号
```js
// 错误写法
var i, a;
for (i = 0; i < 10; i++) {
    var a = document.createElement('a');
    a.innerHTML = i + '<br>';
    a.addEventListener('click', function (e) {
      e.preventDefault();
      alert(i);
    }
    document.body.appendChild(a);
}
// 这样写确实能够创建 10 个 a 标签, 但是当你点击任何一个标签时, 弹出的结果都是 10. 因为他的向外找 i 时, i 已经全部循环完毕变成10.

// 正确写法
var i;
for (i = 0; i < 10; i++) {
  (function(i){
    var a = document.createElement('a');
    a.innerHTML = i + '<br>';
    a.addEventListener('click', function (e) {
      e.preventDefault();
      alert(i);
    })
    document.body.appendChild(a);
  })(i)
}
// 在原本的方法外包裹一层 function, 将 i 传入, 这样就可以避免上面的问题.
```
#### 如何理解作用域
1. 自由变量
2. 作用域链, 即自由变量的查找
3. 闭包的两个场景

#### 实际开发中闭包的应用
闭包实际应用主要用于封装变量, 收敛权限
```js
function isFirstLoad() {
  var _list = [];
  return function (user_id) {
    if (_list.indexOf(user_id) >= 0) {
      return false;
    } else {
      _list.push(user.id);
      return true;
    }
  }
}

//使用
var firstLoad = isFirstLoad();
firstLoad(001)    //true 
firstLoad(001)    //false
firstLoad(002)    //true
// 这样在 isFirstLoad 外, 根本不可能修改 _list 的值.
```