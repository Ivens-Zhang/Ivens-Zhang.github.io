---
layout: post
title: "JavaScript 基础面试题——JS-Web-API"
subtitle: ""
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-11-30/th.jpg"
hidden: true
catalog: true
tags:
  - 前端学习笔记
---

## 正文
### DOM 操作
#### 知识点
- DOM 节点操作
- DOM 结构操作
  - 新增节点

```js
var div1 = document.getElementById('div1');

// 添加新节点
var p1 = document.createElement('p');
p1.innerHTML = 'this is p1';
div1.appendChild(p1);

// 移动已有节点
var p2 = document.getElementById('p2');
div1.appendChild(p2);
```
  - 删除节点

```js
var div1 = document.getElementById('div1');
var child = div1.childNodes;
div1.removeChild(child[0]);
```
  - 获取父元素
  - 获取子元素

```js
var div1 = document.getElementById('div1');
var parent = div1.parentElement;
var child = div1.childNodes;
```
#### 题目
**1.`DOM` 是那种基本的数据结构?**

树

**2.`DOM` 操作的常用 `API` 有哪些?**

1. 获取 DOM 节点, 以及节点的 property 和 attribute
2. 获取父子节点 --- childNodes / parentNode
3. 新增/删除节点

**3.`DOM` 节点的 `attr` 和 `property` 有何区别?**

- `property` 只是一个 JS 对象的属性的修改
- `Attribute` 是对 html 标签属性的修改

```html
<p id="p1" data-name="p1-data-name"></p>

<script>
  var p1 = document.getElementById('p1');
  console.log(p1.getAttribute('data-name');)

  p1.setAttribute('data-name', 'abc');
  console.log(p1.getAttribute('data-name');)
</script>
```
输出结果为:
```
p1-data-name
abc
```


### BOM 操作
#### 知识点
- navigator
- screen

```js
// navigator
var ua = navigator.userAgent;
var isChrome = ua.indexOf('Chrome');
console.log(isChrome);

// screen
console.log(screen.width);
console.log(screen.height);
```
- location
- history

```js
// location
console.log(location.href) 
// 获取当前地址
console.log(location.protocol) 
// 获取当前协议(http / https)
console.log(location.pathname) 
// 获取当前路径名, 域名后的路径
console.log(location.search) 

console.log(location.hash)

// history
history.back() 
histort.forward()
```
### 事件绑定
#### 知识点
- 通用事件绑定
- 事件冒泡

事件冒泡: 当在此元素中找不到对应事件的方法, 则由子节点向父节点去触发同名事件.

可用于一次绑定页面中方法相同的元素.
![](../../../../img/in-post/2019-12-4/a.png)

- 代理

当需要动态添加事件时, 可以使用代理.

在需绑定事件元素的父元素上绑定事件, 通过 `e.target` 获取鼠标事件的目标元素, 再判断是否是那种需要绑定事件的元素.

好处:
1. 代码简洁
2. 减少浏览器内存占用

![](../../../../img/in-post/2019-12-4/b.png)
#### 题目
编写一个通用的事件监听函数
```js
function bindEvent(elem, type, fn) {
  elem.addEventListener(type, fn);
}
```
### AJAX 请求(包括 http 协议)
#### 知识点
- XMLHTTPRequest
- 状态码说明

![](../../../../img/in-post/2019-12-4/c.png)

[常见的HTTP状态码](https://www.runoob.com/http/http-status-codes.html)
- 跨域
  - 可跨域标签
    - `img` 打点统计, 统计网站 
    - `link` 使用外部样式等 
    - `script` 使用 JSONP
  - 服务器端设置 `http header` 

![](../../../../img/in-post/2019-12-4/d.png)
  - 注意事项
    - 所有跨域请求都必须经过信息提供方许可
    - 未经许可就能获取, 是浏览器同源策略出现漏洞



#### 题目
手动编写一个 AJAX
```js
var xhr = new XMLHttpRequest();
xhr.open('Get', '/api',false);
xhr.onreadystatechange = function () {
  if (xhr.readyState == 4) {
    if (xhr.readyStatus == 200) {
      alert(xhr.responseText);
    }
  }
}
xhr.send(null);
```

跨域的几种方法

### 存储
#### 知识点
- cookie
  - 本身用于客户端和服务器端通信
  - 但是有本地存储的功能
  - 使用 `document.cookie = ...` 获取和修改即可
- cookie 的缺点
  - 存储量太小 (4KB)
  - 每次请求都要携带, 会影响效率
  - API 过于简单, 需要封装才能使用  
- localStorage 和 sessionStore
  - H5 专门为存储设计, 最大容量 5M
  - API 简单 `localStore.setItem(key, value)` , `localStore.getItem(key)`

***

**JavaScript 基础面试题 课程到此结束.**