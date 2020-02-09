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

new 和 Object.create 区别

在js中，创建对象有三种方式 ：
1. {}
2. new Object()
3. Object.create()

1和2的区别很小，1不能传参，2可以传参。

## 三. 作用域与作用域链

### 1. 变量提升
**JavaScript 中，变量可以在使用后声明，也就是变量可以先使用再声明。**

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200208225252.png"  />

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
**预编译阶段，js 将其分解为`变量声明`与`变量赋值`。**

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
控制台打印结果 "`function`"。另一段代码:
```js
var f = function () {
  console.log("var");
}
 
function f() {
  console.log("function");
}
f();//=>?
```
控制台打印结果 "`var`"。

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

### 2. 作用域和作用域链

推荐文章: [《 深入理解 JavaScript 作用域和作用域链 》★★★](https://www.cnblogs.com/fundebug/p/10535230.html)

**在 ES6 之前, JavaScript 中只用两种作用域: `全局作用域` 和 `函数作用域` .**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200208232934.png)

没有块级作用域会带来什么情况?

```js
if(true) {
  var name = 'Tom'
}

for(var i = 0; i < 3; i++) {}

console.log(name)		// Tom
console.log(i)		// 2
```

这些原本是块级作用域的变量, 在 JavaScript 中在预编译时会自动声明为全局变量, 这样会造成污染全局命名空间, 容易引起命名冲突。

> 这就是为何 jQuery、Zepto 等库的源码，所有的代码都会放在`(function(){....})()`中。因为放在里面的所有变量，都不会被外泄和暴露，不会污染到外面，不会对其他的库或者 JS 脚本造成影响。这是函数作用域的一个体现。

---

#### 全局作用域和函数作用域

直接看一个例子:

```js
var outVariable = "我是最外层变量"; //最外层变量
function outFun() { //最外层函数
    var inVariable = "内层变量";
    function innerFun() { //内层函数
        console.log(inVariable);
    }
    innerFun();
}
console.log(outVariable); //我是最外层变量
outFun(); //内层变量
console.log(inVariable); //inVariable is not defined
innerFun(); //innerFun is not defined
```

每一个函数中, 命名的变量都是独立的, 只能在本函数内使用, 无法在全局中使用.

- 所有末定义直接赋值的变量自动声明为拥有全局作用域

```js
function outFun2() {
    variable = "未定义直接赋值的变量";
    var inVariable2 = "内层变量2";
}
outFun2();//要先执行这个函数，否则根本不知道里面是啥 ★★★
console.log(variable); //未定义直接赋值的变量
console.log(inVariable2); //inVariable2 is not defined
```

- 所有 `window` 对象的属性拥有全局作用域

---

#### 作用域链的概念

要解释作用域链, 我们首先得解释什么是自由变量.

> 我们可以将自由变量粗略地理解为: 定义在函数父级的变量.

```js
var a = 100
function fn() {
    console.log(a) // 这里的a在这里就是一个自由变量
}
fn()
```

那么什么是作用域链呢?

我们可以理解为: 当函数中需要使用一个变量, 函数会首先在自身内部寻找, 如果找不到就向上到父级中寻找, 直到找到全局变量.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200208235543.png)

---

#### 自由变量的取值问题

```js
var x = 10
function fn() {
  console.log(x)
}
function show(f) {
  var x = 20
  f() //10，而不是20
}
show(fn)
```

在 `fn` 函数中，取自由变量 x 的值时，要到哪个作用域中取？——**要到创建 `fn` 函数的那个作用域中取，无论 `fn` 函数将在哪里调用。**

> **作用域中取值, 这里强调的是 “创建”，而不是 “调用”**，切记切记——其实这就是所谓的 "静态作用域"

---

#### let 和 const 的优点

1. 暂时性死区, 在块级作用域内定义不会被提升为全局变量
2. 不允许重复命名, 否则会报错
3. 循环中方便使用

```js
for(var i = 0; i < 3; i++) {}
console.log(i)		// 2
------------------------------------------
for(let i = 0; i < 3; i++) {}
console.log(i)		// i is not defined
```

```js
<button>测试1</button>
<button>测试2</button>
<button>测试3</button>
<script type="text/javascript">
   var btns = document.getElementsByTagName('button')
    for (var i = 0; i < btns.length; i++) {
      btns[i].onclick = function () {
        console.log('第' + (i + 1) + '个')
      }
    }
</script>
// 点击任意一个按钮，后台都是弹出 “第四个”, 这是因为 i 是全局变量, 执行到点击事件时，此时 i 的值为 3。
```

```js
// 修改方法如下: 
for (let i = 0; i < btns.length; i++) {
  btns[i].onclick = function () {
    console.log('第' + (i + 1) + '个')
  }
}
```









