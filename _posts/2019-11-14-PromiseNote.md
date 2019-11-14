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

***

古人云：“君子一诺千金”，这种“承诺将来会执行”的对象在JavaScript中称为Promise对象。

既然JS是单线程的，那是如何完成所谓的并行任务的呢?

JS引擎是单线程的，但是浏览器是多线程的，其执行的方法如`setTimeOut`是由浏览器来调度的，计时器也是一个单独的线程（并不是JS引擎来计时）。而`Promise.all(iterable)` 方法返回一个 Promise 实例,其内部方法串行执行，比如P1先计时完成，浏览器的触发线程就将P1方法放到JS队列中由JS引擎来执行。

一个最简单的Promise例子:
```js
// 生成一个0-2之间的随机数，如果小于1，则等待一段时间后返回成功，否则返回失败
function test(resolve,reject){
    var timeOut = Math.random() * 2;
    log('set timeout to: ' + timeOut + ' seconds.');
    setTimeout(function(){
        if(timeOut < 1){
            log('call resolve()...');
            resolce('200 OK');
        }
        else{
            log('call reject()...');
            reject('timeout in' + timoeOut + 'seconds.');
        }
    },timeOut * 1000);
}
```