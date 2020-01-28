---
layout: post
title: "Promise学习笔记"
subtitle: ''
author: "Ivens"
catalog: true
header-mask: 0.1
header-img: "img/in-post/2019-11-14/th.jpg"
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

### 既然JS是单线程的，那是如何完成所谓的并行任务的呢?

JS引擎是单线程的，但是浏览器是多线程的，其执行的方法如`setTimeOut`是由浏览器来调度的，计时器也是一个单独的线程（并不是JS引擎来计时）。而`Promise.all(iterable)` 方法返回一个 Promise 实例,其内部方法串行执行，比如P1先计时完成，浏览器的触发线程就将P1方法放到JS队列中由JS引擎来执行。

## 正文

### 总结
先初始化 `Promise`对象在里面设置方法,判断何时`resolve()`何时`reject()`.

然后写`then()`方法,设置如果`resolve()`该运行什么;再写`catch()`方法,设置如果`reject()`该执行什么.

如果是`resolve()`则返回一个新的`Promise`对象,可以继续使用`then()`或者`catch()`.

**另外，分享几篇不错的教程：**

 **[《Promise笔记》](https://segmentfault.com/a/1190000011652907)**

[《MDN文档——Promise》](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)

[《廖雪峰JavaScript教程》](https://www.liaoxuefeng.com/wiki/1022910821149312/1023024413276544#0)

[《ECMAScript 6 —— Promise》](http://es6.ruanyifeng.com/#docs/promise)

[《简书——Promise笔记（很全）》](https://www.jianshu.com/p/1b63a13c2701)

***

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
能看出来,代码并没有按照我们代码的顺序执行,这在一些互不影响的语句中其实没什么,**但是当第二句语句要使用上一句语句的返回值时**,程序就会出现问题.

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
1. `pending`<br>
初始状态,也称为未定状态，就是初始化Promise时，调用executor执行器函数后的状态。
1. `fulfilled`<br>
完成状态，意味着异步操作成功。
3. `rejected`<br>
失败状态，意味着异步操作失败。

它只有两种状态可以转化，即:
- 操作成功<br>
`pending -> fulfilled`
- 操作失败<br>
`pending -> rejected`

并且这个状态转化是`单向的`，不可逆转，已经确定的状态（fulfilled/rejected）无法转回初始状态（pending）。

![](../../../../img/in-post/2019-11-14/e.png)

***

### Promise方法
#### Promise.prototype.then()
`then()`调用后返回一个Promise对象，意味着实例化后的Promise对象可以进行链式调用，而且这个then()方法可以接收两个函数，一个是处理成功后的函数，一个是处理错误结果的函数。
```js
promise1.then(function(data) {
  console.log(data); // success
}, function(err) {
  console.log(err); // 不执行
//   这里第二个function就相当于catc()
})
```

```js
var promise1 = new Promise(function(resolve, reject) {
  // 2秒后置为接收状态
  setTimeout(function() {
    resolve('success');
  }, 2000);
});

promise1.then(function(data) {
  console.log(data); // success
}, function(err) {
  console.log(err); // 不执行
}).then(function(data) {
  // 上一步的then()方法没有返回值
  console.log('链式调用：' + data); // 链式调用：undefined 
}).then(function(data) {
  // ....
});
```
*返回的这个Promise对象的状态主要是根据promise1.then()方法返回的值，大致分为以下几种情况：*

- 如果then()方法调用`resolve()`方法，那么返回的Promise将会变成接收状态。
- 如果then()方法调用`reject()`方法，那么返回的Promise将会变成拒绝状态。
- 如果then()方法中返回了`一个参数值`，那么返回的Promise将会变成接收状态。
- 如果then()方法中抛出了`一个异常`，那么返回的Promise将会变成拒绝状态。
- 如果then()方法返回了`一个未知状态(pending)的Promise新实例`()，那么返回的新Promise就是未知状态。
- 如果then()方法`没有明确指定`的resolve(data)/reject(data)/return data时，那么返回的新Promise就是接收状态，可以一层一层地往下传递。

```js
var promise2 = new Promise(function(resolve, reject) {
  // 2秒后置为接收状态
  setTimeout(function() {
    resolve('success');
  }, 2000);
});

promise2
  .then(function(data) {
    // 上一个then()调用了resolve，置为fulfilled态
    console.log('第一个then');
    console.log(data);
    return '2';
  })
  .then(function(data) {
    // 此时这里的状态也是fulfilled, 因为上一步返回了2
    console.log('第二个then');
    console.log(data);  // 2

    return new Promise(function(resolve, reject) {
      reject('把状态置为rejected error'); // 返回一个rejected的Promise实例
    });
  }, function(err) {
    // error
  })
  .then(function(data) {
    /* 这里不运行 */
    console.log('第三个then');
    console.log(data);
    // ....
  }, function(err) {
    // error回调
    // 此时这里的状态也是fulfilled, 因为上一步使用了reject()来返回值
    console.log('出错：' + err); // 出错：把状态置为rejected error
  })
  .then(function(data) {
    // 没有明确指定返回值，默认返回fulfilled
    console.log('这里是fulfilled态');
});
```

#### Promise.prototype.catch()
`catch()`方法和`then()`方法一样，都会**返回一个新的Promise对象**，它主要用于捕获异步操作时出现的异常。因此，我们通常省略then()方法的第二个参数，把错误处理控制权转交给其后面的catch()函数.

#### Promise.all()
Promise.all()接收一个参数，它必须是可以迭代的，比如数组。它的状态受参数内各个值的状态影响，即里面状态全部为fulfilled时，它才会变成fulfilled，否则变成rejected。这就好比一个`&&`,也像一个过滤器,参数里必须完全正确才能运行后面的`then()`方法.

```js
// 置为fulfilled状态的情况
var arr = [1, 2, 3];
var promises = arr.map(function(e) {
  return new Promise(function(resolve, reject) {
    resolve(e * 5);
  });
});

Promise.all(promises).then(function(data) {
    // 有序输出
  console.log(data); // [5, 10, 15]
  console.log(arr); // [1, 2, 3]
});


// 置为rejected状态的情况
var promises2 = arr.map(function(e) {
  return new Promise(function(resolve, reject) {
    if (e === 3) {
      reject('rejected');
    }
    resolve(e * 5);
  });
});

Promise.all(promises2).then(function(data) {
  // 这里不会执行
  console.log(data);
  console.log(arr);
}).catch(function(err) {
  console.log(err); // rejected
});
```
#### Promise.race()

Promise.race()和Promise.all()类似，都接收一个可以迭代的参数，但是不同之处是Promise.race()的状态变化不是全部受参数内的状态影响，一旦参数内有一个值的状态发生的改变，那么该Promise的状态就是改变的状态。就跟race单词的字面意思一样，谁跑的快谁赢。

```js
var p1 = new Promise(function(resolve, reject) {
  setTimeout(resolve, 300, 'p1 doned');
});

var p2 = new Promise(function(resolve, reject) {
  setTimeout(resolve, 50, 'p2 doned');
});

var p3 = new Promise(function(resolve, reject) {
  setTimeout(reject, 100, 'p3 rejected');
});

Promise.race([p1, p2, p3]).then(function(data) {
  // 显然p2更快，所以状态变成了fulfilled
  // 如果p3更快，那么状态就会变成rejected
  console.log(data); // p2 doned
}).catch(function(err) {
  console.log(err); // 不执行
});
```

#### Promise.resolve()与Promise.reject()
Promise.resolve()接受一个参数值，可以是普通的值，具有then()方法的对象和Promise实例。正常情况下，它返回一个Promise对象，状态为fulfilled。但是，当解析时发生错误时，返回的Promise对象将会置为rejected态。Promise.reject()和Promise.resolve()正好相反，它接收一个参数值reason，即发生异常的原因。此时返回的Promise对象将会置为rejected态。

```js
// 参数为普通值
var p4 = Promise.resolve(5);
p4.then(function(data) {
  console.log(data); // 5
});


// 参数为含有then()方法的对象
var obj = {
  then: function() {
    console.log('obj 里面的then()方法');
  }
};

var p5 = Promise.resolve(obj);
p5.then(function(data) {
  // 这里的值时obj方法里面返回的值
  console.log(data); // obj 里面的then()方法
});


// 参数为Promise实例
var p6 = Promise.resolve(7);
var p7 = Promise.resolve(p6);

p7.then(function(data) {
  // 这里的值时Promise实例返回的值
  console.log(data); // 7
});

// 参数为Promise实例,但参数是rejected态
var p8 = Promise.reject(8);
var p9 = Promise.resolve(p8);

p9.then(function(data) {
  // 这里的值时Promise实例返回的值
  console.log('fulfilled:'+ data); // 不执行
}).catch(function(err) {
  console.log('rejected:' + err); // rejected: 8
});

var p10 = Promise.reject('手动拒绝');
p10.then(function(data) {
  console.log(data); // 这里不会执行，因为是rejected态
}).catch(function(err) {
  console.log(err); // 手动拒绝
}).then(function(data) {
 // 不受上一级影响
  console.log('状态：fulfilled'); // 状态：fulfilled
});
```