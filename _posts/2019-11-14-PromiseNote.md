---
layout: post
title: "Promise学习笔记（一）"
subtitle: ''
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-11-14/th.jpg"
hidden: true
tags:
  - 前端学习笔记
---
- [前言](#%e5%89%8d%e8%a8%80)
  - [什么是并行与并发?](#%e4%bb%80%e4%b9%88%e6%98%af%e5%b9%b6%e8%a1%8c%e4%b8%8e%e5%b9%b6%e5%8f%91)
  - [既然JS是单线程的，那是如何完成所谓的并行任务的呢?](#%e6%97%a2%e7%84%b6js%e6%98%af%e5%8d%95%e7%ba%bf%e7%a8%8b%e7%9a%84%e9%82%a3%e6%98%af%e5%a6%82%e4%bd%95%e5%ae%8c%e6%88%90%e6%89%80%e8%b0%93%e7%9a%84%e5%b9%b6%e8%a1%8c%e4%bb%bb%e5%8a%a1%e7%9a%84%e5%91%a2)
- [正文](#%e6%ad%a3%e6%96%87)
  - [Promise是用来解决什么问题的?](#promise%e6%98%af%e7%94%a8%e6%9d%a5%e8%a7%a3%e5%86%b3%e4%bb%80%e4%b9%88%e9%97%ae%e9%a2%98%e7%9a%84)
  - [有没有更好的写法？](#%e6%9c%89%e6%b2%a1%e6%9c%89%e6%9b%b4%e5%a5%bd%e7%9a%84%e5%86%99%e6%b3%95)
  - [没有方法再次改进代码?](#%e6%b2%a1%e6%9c%89%e6%96%b9%e6%b3%95%e5%86%8d%e6%ac%a1%e6%94%b9%e8%bf%9b%e4%bb%a3%e7%a0%81)

## 前言

**在学习`Promise`之前有个问题需要弄明白.**

### 什么是并行与并发?

这个问题困扰了我很久,今天花点时间弄清楚.

- 并行:指两个或者多个事件在同一时刻发生,是在多台处理器上同时处理多个任务
- 并发:指两个或多个事件在同一时间间隔发生,是在一台处理器上“同时”处理多个任务

![](../../../../img/in-post/2019-11-14/b.png)
<center><strong>并行</strong></center>

![](../../../../img/in-post/2019-11-14/c.png)
<center><strong>并发</strong></center>

***

当有多个线程在操作时，**如果系统只有一个 CPU**，则它根本不可能真正同时进行一个以上的线程，它只能把 CPU 运行时间划分成若干个时间段，再将时间段分配给各个线程执行，在一个时间段的线程代码运行时,其它线程处于挂起状态.这种方式我们称之为并发（Concurrent）。

**当系统有一个以上 CPU 时**，则线程的操作有可能非并发。当一个 CPU 执行一个线程时，另一个 CPU 可以执行另一个线程，两个线程互不抢占 CPU 资源，可以同时进行，这种方式我们称之为并行（Parallel）。

![](../../../../img/in-post/2019-11-14/d.png)

### 既然JS是单线程的，那是如何完成所谓的并行任务的呢?

JS引擎是单线程的，但是浏览器是多线程的，其执行的方法如`setTimeOut`是由浏览器来调度的，计时器也是一个单独的线程（并不是JS引擎来计时）。而`Promise.all(iterable)` 方法返回一个 Promise 实例,其内部方法串行执行，比如P1先计时完成，浏览器的触发线程就将P1方法放到JS队列中由JS引擎来执行。

## 正文

先分一篇不错的笔记—— **[《Promise笔记》](https://segmentfault.com/a/1190000011652907)**

### Promise是用来解决什么问题的?

**先看这段代码:**
```js
function callback() {
    console.log('Done');
}
console.log('before setTimeout()');
setTimeout(callback, 1000); // 1秒钟后调用callback函数
console.log('after setTimeout()');
```
**在Chrome的控制台输出可以看到：**
```
before setTimeout()
after setTimeout()
(等待1秒后)
Done
```
能看出来,代码并没有按照我们的顺序执行,这在一些互不影响的语句中其实没什么,**但是当第二句语句要使用上一句语句的返回值时**,程序就会出现问题.

### 有没有更好的写法？
**AJAX就是典型的异步操作：**
```js
request.onreadystatechange = function () {
    if (request.readyState === 4) {
        if (request.status === 200) {
            return success(request.responseText);
        } else {
            return fail(request.status);
        }
    }
}
```
我们可以通过判断`当出现什么情况时执行对应操作`,去让程序按照我们要求的顺序执行.

可是,[廖雪峰](https://www.liaoxuefeng.com/wiki/1022910821149312/1023024413276544#0)前辈的看法是:"**把回调函数success(request.responseText)和fail(request.status)写到一个AJAX操作里很正常，但是`不好看`，而且`不利于代码复用`**"。

### 没有方法再次改进代码?
```js
var ajax = ajaxGet('http://...');
ajax.ifSuccess(success)
    .ifFail(fail);
```
这种`链式写法`的好处在于:
1. 先统一执行AJAX逻辑,不关心如何处理结果
2. 根据结果是成功还是失败，在将来的某个时候调用`success`函数或`fail`函数。

**`Promise `对象用于一个异步操作的最终完成（或失败）及其结果值的表示。简单点说，它就是用于处理异步操作的，异步处理成功了就执行成功的操作，异步处理失败了就捕获错误或者停止后续操作。**

```js
// 它的一般表示形式
new Promise(
    /* executor */
    function(resolve, reject) {
        if (/* success */) {
            // ...执行代码
            resolve();
        } else { /* fail */
            // ...执行代码
            reject();
        }
    }
);
```

如果异步操作**成功**，则可以调用`resolve()`来将该实例的状态置为`fulfilled`，即已完成的，如果一旦**失败**，可以调用`reject()`来将该实例的状态置为`rejected`，即失败的。

Promise对象有三种状态：
1. pending<br>
初始状态,也称为未定状态，就是初始化Promise时，调用executor执行器函数后的状态。
1. fulfilled<br>
完成状态，意味着异步操作成功。
3. rejected<br>
失败状态，意味着异步操作失败。