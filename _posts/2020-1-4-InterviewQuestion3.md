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

```js
// 输出以下方法执行的结果
function Foo () {
  this.a = function () {
    console.log('1')
  }
  Foo.a = function () {
    console.log('2')
  }
}
Foo.prototype.a = function () {
  console.log('3')
}
Foo.a = function () {
  console.log('4')
}
Foo.a()		// 这里直接使用 Foo 对象自带的 a 方法, 结果为: 4
var obj = new Foo()		// 将 Foo 作为类生成对象 obj, 将 a 方法绑定 obj 对象, 替换原有的 Foo 类中的 a 方法.
obj.a()		// 执行 obj 对象 a 方法, 结果为: 1
Foo.a()		// 执行 Foo 类中的 a 方法, 结果为: 2
```





### JavaScript 中继承是怎么实现的 ?

*请参考 :* [*《 JavaScript实现继承 》*](https://segmentfault.com/a/1190000016184431#item-5)

`new` 关键字的原理

1. 执行, 形成函数作用域, 形参赋值, 变量提升
2. 创建一个对象, 将对象的 `__proto__` 绑定类的 `prototype`, 将类中函数的 `this` 执行新建的对象.
3. 返回创建的对象

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

---

#### 闭包的概念

>闭包是指有权访问另一个函数作用域中的变量的函数 --《JavaScript高级程序设计》

> 函数对象可以通过作用域关联起来，函数体内的变量都可以保存在函数作用域内，这在计算机科学文献中称为“闭包”,所有的javascirpt函数都是闭包 --《Javascript权威指南》

上文是两本权威书籍对`闭包(Closures)` 的解释, 我举一个简单的例子便于理解:

```js
function fn () {
  var a = 1;
}
```

这是一个简单的函数声明, 在函数内部定义了一个变量 `a`, 这时有一个问题, 如果我们要在函数外部调用这个变量 `a` 该怎么实现呢?

有两种方法:

1. 直接在 `fn()` 中定义 `return a`
2. 使用闭包

```js
// 使用闭包将函数作用域中的变量让外界调用
function fn () {
  var a = 1;
  function fn2 () {
    return a
  }
  return fn2()
}
console.log(fn())
```

这样我们就可以在外部直接使用函数内部的变量了. 可以这样理解: 闭包就是函数作用域与外部连接的桥梁.

闭包最大的特点: 可以 "记住" 创建时的环境.  (这点可以参考上文 "自由变量" 到创建 `fn` 函数的那个作用域中取值)

---

#### 闭包的使用

**一个计数器demo**

```js
// 如果我们不使用闭包的方式:
function count() {
  var start = 0
  start ++
  return start
}
console.log(count())		// 1
console.log(count())		// 1
// 为什么会出现这种情况呢?
// 因为我们每次在全局调用这个函数, 里面的 start 变量都会重新定义一遍. 也就是说每次我们希望 start 可以在原有的基础上 +1, 其实都是重新从 0 开始 +1.
```

```js
function count() {
  var start = 0
  function addCount() {
    return start ++		// 这里的 return start ++ 等于 return start; start ++
  }
  return addCount
}
var a = count()
console.log(a())		// 0
console.log(a())		// 1
console.log(a())		// 2
```

**一个 get set 的闭包实现**

```js
function Student(name) {
  var name = name
  function setName(name1) {
    name = name1
  }
  function getName() {
    return name
  }
  return{
    setName,
    getName
  }
}
var s1 = Student('Tom')
console.log(s1.getName());		// Tom
s1.setName('Jack')
console.log(s1.getName());		// Jack
```

---

#### this 的定义

推荐文章： [《 深入理解 JavaScript 中的 this 》](http://caibaojian.com/deep-in-javascript-this.html)

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200210154031.png"  />

**第一, JavaScript 中的 `this` 与传统面向对象编程语言 (如: JAVA, C#) 中的 `this` 其实不是一个概念. 在 JavaScript 中:**

> `this` 是一个指针, 指向我们调用函数的对象.

```js
var person = {
  name: 'Jack',
  print: function() {
    console.log(this.name)
  }
}
person.print()		// Jack
var print = person.print
print()						// undefined
```

由上面的代码我们可以看出: 

> `this` 的值并不是在 "创建" 时就被写死的, 而是根据在被调用时, 是哪个对象对该函数进行调用决定的.

再来一个例子:

```js
var name = "Jay Global";
var person = {
    name: 'Jay Person',
    details: {
        name: 'Jay Details',
        print: function() {
            return this.name;
        }
    },
    print: function() {
        return this.name;
    }
};
console.log(person.details.print());  // Jay Details
console.log(person.print());          // Jay Person
var name1 = person.print;
var name2 = person.details;
console.log(name1()); // Jay Global
console.log(name2.print()) // Jay Details, 因为这是由 name2 对象调用的函数, 而 name2 指向 person.details
```

回调函数中的 `this` 一般都是 `window`.

---

#### 箭头函数

[《 ES6 入门 —— 箭头函数 》](http://es6.ruanyifeng.com/#docs/function#箭头函数)

在箭头函数中, 他的 `this` 是函数定义时所属上下文的 `this`, 例如定义在最外层, 则 `this = window`, 且无法使用 `call, apply, bind` 等方法进行修改.

箭头函数无法被 `new`, 因为箭头函数没有 `this` 和 `prototype`.

---

#### call、apply、bind 的使用

> 上面这三种方法都是为了改变 `this` 的指向. 

**JavaScript call( ) 的使用方法:**

```js
var obj = {
    name: 'Tom',
    print: function () {
        console.log(this.name);
    }
}
var obj2 = {
  name: 'Jack'
}
obj.print.call(obj2)		// Jack
obj.print.call({name: 'Lucy'})		// Lucy
```

**带参数的 call( ) 方法**

```js
var person = {
  fullName: function(city, country) {
    return this.firstName + " " + this.lastName + "," + city + "," + country;
  }
}
var person1 = {
  firstName:"Bill",
  lastName: "Gates"
}
person.fullName.call(person1, "Seattle", "USA");		// Bill Gates Seattle USA
```

**apply、call 的区别**

> 两者作用一样, 只是传参的方式不同

```js
fun.call(this, arg1, arg2)
fun.apply(this, [arg1, arg2])
```

如果携带的参数在三个之内, `call` 与 `apply` 性能基本一致, 如果超过 3 个, `call` 的性能更优秀. (JQuery 源码中作者也提到这个概念)

如果要将一个数组作为参数传入 `call` 中该如何写?

```js
let arr = [1,2,3]
function fn (x,y,z) {}
fn.call(obj, arr)		// 如果我们这样写, 那相当于 x = [1,2,3], y = z = undefined
fn.call(obj, ...arr)		// 这样才是正确的写法.
```



**bind 与 apply 和 call 的区别**

> bind() 方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind() 方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。—— MDN

也就是说，使用 bind 后我们还需要再调用一下函数。

```js
obj.print.bind({name: 'Tom'})()
```

---

**总结:**

这三个方法携带的参数, 第一个是指定新的 `this` 指向, 后面的参数是传入绑定的方法中的.

举例:

```js
var obj = {
  name: 'Tom',
  sayHello: function (liveCity) {  
    console.log("I'm " + this.name + ". I'm live in " + liveCity)
  }
}

TomSayHello.call({name: 'Jack'}, '上海')		// I'm Jack. I'm live in 上海
TomSayHello.call(null, '上海')							// I'm . I'm live in 上海
TomSayHello.call({name:'Jack'})						 // I'm Jack. I'm live in undefined
```



## DOM 与 BOM 

### 简述 —— DOM

> 当网页被加载时，浏览器会创建页面的文档对象模型（Document Object Model）。
>
> “W3C 文档对象模型（DOM）是中立于平台和语言的接口，它允许程序和脚本动态地访问、更新文档的内容、结构和样式。”

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200210204221.png)

---

### 查找 HTML 元素

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200210212645.png)

[《 查找 HTML 元素 》—— W3School](https://www.w3school.com.cn/js/js_htmldom_elements.asp)

---

### DOM 常用方法

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200211214141.png)

---

### DOM 元素增删

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200212144651.png)

---

### HTML DOM 集合

`getElementByTagName()` 方法返回的是 `HTMLCollection` 对象, 这并不是**一个数组**!

我们可以使用列表遍历这个对象, 但是无法使用数组方法, 如: `valueOf()`、 `pop()`、`push()` 等。

---

### 简述 —— BOM

> 浏览器对象模型（Browser Object Model (BOM)）**允许 JavaScript 与浏览器对话**。
>
> 现代的浏览器已经（几乎）实现了 JavaScript 交互相同的方法和属性，因此它经常作为 BOM 的方法和属性被提到。

---

### Window Screen 对象

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200212171926.png)

可用宽高指的是屏幕宽高减去诸如窗口工具条等界面特征后得到的宽高.

>`screen.colorDepth` 属性返回用于显示一种颜色的比特数。
>
>所有现代计算机都使用 24 位或 32 位硬件的色彩分辨率：
>
>- 24 bits =16,777,216 种不同的 "True Colors"
>- 32 bits = 4,294,967,296 中不同的 "Deep Colors"
>
>更老的计算机使用 14 位：65,536 种不同的 "High Colors" 分辨率。
>
>异常古老的计算机，以及老式的手机使用 8 位：256 中不同的 "VGA colors"。

---

### Window Location 对象

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200212173705.png)

---

### Window History 对象

`History` 对象无法进行写入.

为了保护用户隐私, JavaScript 访问 History 存在限制

常用方法:

- history.back( )			回退
- history.forward( )    前进 

---

### Window Navigator 对象

常用方法:

- navigator.appName		返回浏览器的应用程序名称
- navigator.platform        返回用户当前使用的操作系统
- navigator.appVersion    返回浏览器版本

> 来自 navigator 对象的信息**通常是误导性的，不应该用于检测浏览器版本**，因为：
>
> - 不同浏览器能够使用相同名称
> - 导航数据可被浏览器拥有者更改
> - 某些浏览器会错误标识自身以绕过站点测试
> - 浏览器无法报告发布晚于浏览器的新操作系统

---

### 三种弹出框

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200212213658.png)

---

### `setTimeout` 与 `setInterval`

- setTimeout(*function*, *milliseconds*)

**在等待指定的毫秒数后执行函数。**

- setInterval(*function*, *milliseconds*)

**等同于 setTimeout()，但持续重复执行该函数。**

`clearTimeout()` 和 `clearInterval()` 是停止执行以上方法. 带的参数是之前函数表达式中定义的变量, 如:

```js
var fn = setInterval(function () {
	var d = new Date();
  document.body = d.toLocaleTimeString();
}, 1000)
clearInterval(fn)		// 括号中带之前方法声明的变量
```

---

### `Cookie` & `LocalStorage` & `SessionStore`

- cookie
  - 本身用于客户端和服务器端通信
  - 但是有本地存储的功能
  - 使用 `document.cookie = ...` 获取和修改即可
  - [JavaScript Cookies —— W3School](https://www.w3school.com.cn/js/js_cookies.asp)
- cookie 的缺点
  - 存储量太小 (4KB)
  - 每次请求都要携带, 会影响效率
  - API 过于简单, 需要封装才能使用
- localStorage 和 sessionStore
  - H5 专门为存储设计, 最大容量 5M
  - API 简单 `localStore.setItem(key, value)` , `localStore.getItem(key)`

---

## 事件

### `事件冒泡` 与 `事件捕获`

DOM 事件流 (event flow) 存在三个阶段: **事件捕获阶段** 、 **处于目标阶段** 、 **事件冒泡阶段。**

**事件冒泡（*dubbed bubbling*）：** 当触发 DOM 事件浏览器会从根节点开始**由内到外**进行事件传播，即点击了子元素，先触发了子元素的绑定事件，再逐级触发父元素的绑定事件。

**事件捕获（*event capturing*）：**  与事件冒泡相反，先触发父元素的绑定事件， 然后再触发子元素的事件。

---

***addEventListener(event, listener, useCapture)***　　

- *·参数定义：*event---（事件名称，如click，不带on），listener---事件监听函数，*useCapture---*是否采用事件捕获进行事件捕捉，默认为false，即采用事件冒泡方式

- addEventListener在 IE11、Chrome 、Firefox、Safari等浏览器都得到支持。

---

### 事件冒泡:

```js
parent.addEventListener("click",function(e){
  console.log("click-parent");
},false);

child.addEventListener("click",function(e){
  console.log("click-child");
},false);
```

当我每次点击子元素 `child` 时, 控制台都会打印:

```shell
click-child
click-parent
```

**如果点击子元素不想触发父级元素的事件怎么办? 那就是停止事件传播 —— event.stopPropagation( )**

```js
child.addEventListener("click",function(e){
　　console.log("click-child");
  　e.stopPropagation();
},false);
```

---

### 事件捕获:

与 `事件冒泡` 相反, 将 `addEventListener(event, listener, useCapture)` 的第三个参数设为 `true` 即可.

同样的代码点击子元素 `child` 时得到的结果为:

```shell
click-parent
click-child
```

---

## 内置对象

### Array

**Array 常用方法**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200213235341.png)

### `splice()`语法

```
array.splice(index,howmany,item1,.....,itemX)
```

| 参数              | 描述                                                         |
| :---------------- | :----------------------------------------------------------- |
| index             | 必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。 |
| howmany           | 必需。要删除的项目数量。如果设置为 0，则不会删除项目。       |
| item1, ..., itemX | 可选。向数组添加的新项目。                                   |

### `toString()` 与 `join()`

使用 `toString` 返回的是以逗号间隔的数组拼接成的字符串

`join` 与它的作用相同, 只不过 `join` 可以规定分隔符

---

### `concat()` 语法

`concat()` 方法通过合并（连接）现有数组来创建一个新数组, 之前的两个数组并不会发生改变.

```js
var arr = arr1.concat(arr2, arr3);   // 结果为: arr1 + arr2 + arr3
```

---

### `flat()` 语法

`flat()` 方法会按照一个可指定的**深度递归遍历**数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

```js
var newArray = arr.flat(depth)
```

`depth` 可选, 指定要提取嵌套数组的结构深度，默认值为 1。

如果要完全遍历数组, 可以将 `depth` 定义为 `Infinity`.

```js
//使用 Infinity，可展开任意深度的嵌套数组
var arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);		// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

## Math 对象常用方法

| 方法                                                         | 描述                         |
| :----------------------------------------------------------- | :--------------------------- |
| [abs(x)](https://www.w3school.com.cn/jsref/jsref_abs.asp)    | 返回数的绝对值。             |
| [ceil(x)](https://www.w3school.com.cn/jsref/jsref_ceil.asp)  | 对数进行上舍入。             |
| [floor(x)](https://www.w3school.com.cn/jsref/jsref_floor.asp) | 对数进行下舍入。             |
| [max(x,y)](https://www.w3school.com.cn/jsref/jsref_max.asp)  | 返回 x 和 y 中的最高值。     |
| [min(x,y)](https://www.w3school.com.cn/jsref/jsref_min.asp)  | 返回 x 和 y 中的最低值。     |
| [random()](https://www.w3school.com.cn/jsref/jsref_random.asp) | 返回 0 ~ 1 之间的随机数。    |
| [round(x)](https://www.w3school.com.cn/jsref/jsref_round.asp) | 把数四舍五入为最接近的整数。 |

## Generator 函数

> Generator函数时ES6提供的一种异步编程解决方案。Generator语法行为和普通函数完全不同，我们可以把Generator理解为一个**包含了多个内部状态的状态机**。
>
> 执行Generator函数回返回一个**遍历器对象**，也就是说Generator函数除了提供状态机，还可以生成遍历器对象。Generator可以此返回多个遍历器对象，通过这个对象可以访问到Generator函数内部的多个状态。

`Generator` 函数返回一个遍历器对象, 这个对象有一个 `next()` 方法, 执行该方法会返回一个对象, 这个对象带有两个属性: `value` 和 `done`.

```js
function* helloWorldGenerator () {
    yield 'hello';
    yield 'world';
    return 'ending';
}

var hw = helloWorldGenerator();

console.log(hw.next()); 
//第一次调用，Generator函数开始执行，直到遇到yield表达式为止。next方法返回一个对象，它的value属性就是当前yield语句后面表达式的值hello，done属性为false，表示遍历还没有结束

console.log(hw.next()); 
//第二次调用，Generator函数从上次yield表达式停下的地方，一直执行到下一个yield表达式。next方法返回的对象的value属性就是当前yield语句后面表达式的值world，done属性值为false，表示遍历还没有结束。

console.log(hw.next()); 
//第三次调用，Generator函数从上次yield表达式停下的地方，一直执行到return语句（如果没有return语句，则value属性为undefined），done属性为true，表示遍历已经执行结束。

console.log(hw.next()); 
//第四次调用，此时Generator函数已经执行完毕，next方法返回对戏那个的value属性为undefined，done属性为true，表示遍历结束。

console.log(hw.next()); 
//第五次执行和第四次执行的结果是一样的。 
```

执行结果如下:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200214205745.png)

## 解构赋值

解构赋值是对赋值运算符的拓展.

它是一种针对数组或者对象进行模式匹配, 然后对其中的变量进行赋值.

在代码书写上简洁且易读, 语义更清晰明了, 也方便复杂对象中数据字段获取.

---

- 等于号的两边要对应:

```js
var [a,b,c] = [1,2,3]
console.log(a,b,c);		// 1 2 3
```

- 也可以是嵌套结构:

```js
let [a, [[b], c]] = [1, [[2], 3]];
// a = 1
// b = 2
// c = 3
```

- 也可以忽略

```js
let [a, , b] = [1, 2, 3];
// a = 1
// b = 3
```

- 也可以不完全解构

```js
let [a = 1, b] = []; 
// a = 1, b = undefined
```

- 字符串

```js
let [a, b, c, d, e] = 'hello';
// a = 'h'
// b = 'e'
// c = 'l'
// d = 'l'
// e = 'o'
```

- 当解构模式有匹配结果，且匹配结果是 `undefined` 时，会触发默认值作为返回结果。

```js
let [a = 2] = [undefined]; // a = 2
let [a = 3, b = a] = [];     // a = 3, b = 3
let [a = 3, b = a] = [1];    // a = 1, b = 1
let [a = 3, b = a] = [1, 2]; // a = 1, b = 2
```

---

## 对象模型的解构（Object）

**基本**

```js
let { foo, bar } = { foo: 'aaa', bar: 'bbb' }; 
// foo = 'aaa' // bar = 'bbb'  

let { baz : foo } = { baz : 'ddd' }; 
// foo = 'ddd'
```

**可嵌套可忽略**

```js
let obj = {p: ['hello', {y: 'world'}] }; 
let {p: [x, { y }] } = obj; 
// x = 'hello' 
// y = 'world' 

let obj = {p: ['hello', {y: 'world'}] }; 
let {p: [x, {  }] } = obj; 
// x = 'hello'
```

**不完全解构**

```js
let obj = {p: [{y: 'world'}] }; 
let {p: [{ y }, x ] } = obj; 
// x = undefined 
// y = 'world'
```

**剩余运算符**

```js
let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40}; 
// a = 10 
// b = 20 
// rest = {c: 30, d: 40}
```

**解构默认值**

```js
let {a = 10, b = 5} = {a: 3}; 
// a = 3; b = 5; 

let {a: aa = 10, b: bb = 5} = {a: 3}; 
// aa = 3; bb = 5;
```

个人感觉这种赋值方式在配对上有点玄学. 用好了可能会事半功倍, 用不好还不如不用.

## 浏览器

### 为什么要了解浏览器加载、解析、渲染这个过程？

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200215183655.png)

DOM 树的构建过程是一个深度遍历过程：当前节点的所有子节点都构建好后才会去构建当前节点的下一个兄弟节点。

 

### `reflow` 和 `repaint`

1. **reflow (回流): 在浏览器渲染的过程中, 发现某个部分发生变化影响了布局, 那就需要重新渲染.**

2. **repaint (重绘): 某个元素仅改变了背景色, 文字颜色等不影响布局的属性, 浏览器会将发生变化的那部分重新渲染.**

###  出现 `reflow` 的情况

1. 页面初始化时
2. 操作 DOM 时
3. 某些元素尺寸发生变化
4. 部分 CSS 属性发生变化

### 如何减少 `reflow` / `repaint` ?

1. 复用 CSS 样式, 如果要整体修改元素样式, 应该提前在 CSS 文件中规定好修改后的样式, 然后修改元素的 `className` 指向新的样式.
2. 不要在循环中使用 DOM 节点
3. 减少使用 `table` 布局, 因为一个小改动就会使整个表格重新布局
4. 使用 CSS 或 JS 动画时, 提前将元素的 `position` 设置为: `fixed` 或 `absolute`, 那么当他们发生变化时布局不会出现变动.

---

### **[HTML页面加载和解析流程](http://blog.csdn.net/xifeijian/article/details/10813339)**

> 1. 用户输入网址（假设是个html页面，并且是第一次访问），浏览器向服务器发出请求，服务器返回html文件；
> 2. 浏览器开始载入html代码，发现＜head＞标签内有一个＜link＞标签引用外部CSS文件；
> 3. 浏览器又发出CSS文件的请求，服务器返回这个CSS文件；
> 4. 浏览器继续载入html中＜body＞部分的代码，并且CSS文件已经拿到手了，可以开始渲染页面了；
> 5. 浏览器在代码中发现一个＜img＞标签引用了一张图片，向服务器发出请求。此时浏览器不会等到图片下载完，而是继续渲染后面的代码；
> 6. 服务器返回图片文件，由于图片占用了一定面积，影响了后面段落的排布，因此浏览器需要回过头来重新渲染这部分代码；
> 7. 浏览器发现了一个包含一行Javascript代码的＜script＞标签，赶快运行它；
> 8. Javascript脚本执行了这条语句，它命令浏览器隐藏掉代码中的某个＜div＞ （style.display=”none”）。突然少了这么一个元素，浏览器不得不重新渲染这部分代码；
> 9. 终于等到了＜/html＞的到来，浏览器泪流满面……
> 10. 等等，还没完，用户点了一下界面中的“换肤”按钮，Javascript让浏览器换了一下＜link＞标签的CSS路径；
> 11. 浏览器召集了在座的各位＜div＞＜span＞＜ul＞＜li＞们，“大伙儿收拾收拾行李，咱得重新来过……”，浏览器向服务器请求了新的CSS文件，重新渲染页面。

### 启示

**在编写 CSS 时注意:**

1. 尽量减少 DOM 深度
2. 如果使用 id 选择器, 后面就不要跟随类名或标签名了, 因为 id 是一个唯一元素
3. 少用 `*` 通配符
4. 避免后代选择符, 尽量使用子选择符, 如: `#div>p{}`



**Javascript 的加载和执行的特点：**
（1）载入后马上执行；
（2）执行时会阻塞页面后续的内容（包括页面的渲染、其它资源的下载）。原因：因为浏览器需要一个稳定的DOM树结构，而JS中很有可能有 代码直接改变了DOM树结构，比如使用 document.write 或 appendChild,甚至是直接使用的location.href进行跳转，浏览器为了防止出现JS修 改DOM树，需要重新构建DOM树的情况，所以 就会阻塞其他的下载和呈现。

**在编写 JavaScript 时注意:**

1. 将所有的 `script` 标签放到 body 底部
2. 减少 `script` 标签使用次数



参考文章:

[《 浏览器加载、解析、渲染的过程 》](https://blog.csdn.net/XIAOZHUXMEN/article/details/52014901)

---

### 浏览器的同源策略

如果两个页面的协议，端口（如果有指定）和主机都相同，则两个页面具有相同的**源**。我们也可以把它称为“`协议/主机/端口 tuple`”，或简单地叫做“tuple". ("tuple" ，“元”，是指一些事物组合在一起形成一个整体，比如（1，2）叫二元，（1，2，3）叫三元)

|          | URL                                        | 结果 | 原因                           |
| -------- | :----------------------------------------- | :--- | :----------------------------- |
| 初始网址 | `http://store.company.com/dir/page.html`   |      |                                |
| 示例一   | `http://store.company.com/dir2/other.html` | 成功 | 只有路径不同                   |
| 示例二   | `https://store.company.com/secure.html`    | 失败 | 不同协议 ( https和http )       |
| 示例三   | `http://store.company.com:81/dir/etc.html` | 失败 | 不同端口 ( http:// 80是默认的) |
| 示例四   | `http://news.company.com/dir/other.html`   | 失败 | 不同域名 ( news和store )       |

### 什么是 AJAX 跨域?

这是因为浏览器的同源策略导致的。默认情况下，JavaScript 在发送 AJAX 请求时，**URL 的域名必须和当前页面完全一致**。

完全一致的意思是:

- 域名要相同（www.example.com 和 example.com 不同）
- 协议要相同（http 和 https 不同）
- 端口号要相同（默认是: 80 端口，它和: 8080 就不同）。有的浏览器口子松一点，允许端口不同，大多数浏览器都会严格遵守这个限制。

## ES6

### 变量

**`var` 的缺点**

1. 可以重复定义
2. 没有块级作用域
3. 可以随意修改

所以在 ES6 中引入了两种新的定义方式: `let` 和 `const`

|       |              |                 |              |
| ----- | ------------ | --------------- | ------------ |
| var   | 变量提升     | 可以修改        | 可以重复定义 |
| let   | 不可变量提升 | 可以修改        | 不可重复定义 |
| const | 不可变量提升 | 常量, 不可修改★ | 不可重复定义 |

---

### 函数

#### 剩余参数

剩余参数的作用:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200218225617.png)

```js
// 1. 收集参数
function show(a, b, ...arg) {
  console.log(...arg);
}
show(1, 2, 4, 8, 16, 32)	// 结果: 4, 8, 16, 32

// 2. 数组展开
let arr1 = [1,2,3]
		arr2 = [4,5,6]
let arrAdd = [...arr1, ...arr2]
console.log(arrAdd)		// 结果: [1,2,3,4,5,6]
```

#### 默认参数

```js
function sayHello(name, age, city = '上海') {
  console.log(`Hello, my name is ${name}, I'm ${age} years old and I'm living in ${city}`)
}
sayHello('Tom', 12)
// res: Hello, my name is Tom, I'm 12 years old and I'm living in 上海
sayHello('Tom',12, 'Beijing')		
// res: Hello, my name is Tom, I'm 12 years old and I'm living in Beijing
```

### 数组

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200218230536.png)

**map 映射**

一般用于两个数组进行关联映射

```js
var score = [23, 76, 34, 89, 92],
    grade = ['及格', '不及格']
var result = score.map(item => {
  return item < 60 ? grade[1] : grade[0]
})
console.log(result);		// res: [ '不及格', '及格', '不及格', '及格', '及格' ]
```

---

**reduce 汇总**

一般用于对数组的求和, 取平均, 最后生成一个结果.

```js
// 取 5 名学生的身高平均数
let stus = [156, 174, 184, 168, 192],
		avgHeight
avgHeight = stus.reduce((temp, current, index) => {
  return index === stus.length - 1 ? (temp + current) / stus.length : temp + current
})
console.log(avgHeight)
```

---

**filter 过滤器**

用如其名, 用来筛选数组的.

```js
// 返回数组中所有奇数
var arr = [22,49,23,56,71],
		res
res = arr.filter(item => {
  return item % 2 === 1
})
console.log(res)
```

---

**forEach 迭代**

简单遍历

```js
var stus = ['Tom', 'Jack', 'David']
stus.forEach((item, index) => {
  console.log(`学号: ${index + 1}, 姓名: ${item}`);
})
// 学号: 1, 姓名: Tom
// 学号: 2, 姓名: Jack
// 学号: 3, 姓名: David
```

---

**keys/ values/ entries**

ES6 提供三个新的方法——`entries()`，`keys()`和`values()`——用于遍历数组。它们都返回一个遍历器对象（详见《Iterator》一章），可以用`for...of`循环进行遍历，唯一的区别是`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历。

```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

如果不使用`for...of`循环，可以手动调用遍历器对象的`next`方法，进行遍历。

```javascript
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']
console.log(entries.next().value); // [1, 'b']
console.log(entries.next().value); // [2, 'c']
```

---

**`for..in` & `for..of`**

|         | 数组  | Json |
| ------- | ----- | ---- |
| for..in | index | √    |
| for..of | value | ×    |

*`for..of` 是为了靠近 Java 做出的调整, 内部为 迭代器.*

### 字符串

**`startsWith` & `endsWith`**

参数为字符串, 判断字符串开头结尾是否为特定字符串, 返回布尔值.

### 面向对象

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200219212529.png)

**JavaScript 中的面向对象并非真实的面向对象, 而是提供的语法糖.**

```js
--------------------------------------------------------
// 新版本写法
--------------------------------------------------------
class Stu {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  sayHello() {
    console.log(`name: ${this.name}, age: ${this.age}`);
  }
}

var s1 = new Stu('tom', 22)
s1.sayHello()		// name: tom, age: 22

class OldStu extends Stu {
  constructor(name, age, gender) {
    super(name, age)
    this.gender = gender
  }
  ImOld () {
    console.log(`我是${this.age}岁的${this.gender}生`);
  }
}

var os1 = new OldStu('LiLi', 15, '女')
os1.ImOld()		// 我是15岁的女生

--------------------------------------------------------
// 老版本写法
--------------------------------------------------------
function User(name, pwd) {
  this.name = name
  this.pwd = pwd
}
User.prototype.sayHello = function () {  
  console.log(this.name, this.pwd);
}

var user1 = new User('Tom', 'a123')
user1.sayHello()
```

### 消除异步的三种方法:

回调函数 - 简单, 但是容易出现回调地狱

Promise - 如果加入过多逻辑语句会臃肿

Generator - 适合加入逻辑语句

async/await - Generator 的语法糖, 省去 .next() 步骤



- [ ] 需要系统复习一下正则

 

