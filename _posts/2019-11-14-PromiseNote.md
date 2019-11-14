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

古人云：“君子一诺千金”，这种“承诺将来会执行”的对象在JavaScript中称为Promise对象。

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