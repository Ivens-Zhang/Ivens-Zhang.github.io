---
layout: post
title: "Axios学习笔记"
subtitle: ''
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-11-14/th.jpg"
tags:
  - 前端学习笔记
---
## 前言
这两天从promise看到ajax，虽笼统来说就看了两块，但是为了弄懂这两个东西又看了很多包括：
- 同步异步
- 并行并发
- 回调函数

## 正文

[**《axios官方中文文档》**](http://www.axios-js.com/zh-cn/docs/)

### 什么是Axios?

**`Axios` 是一个基于 `promise` 的 HTTP 库，可以用在浏览器和 node.js 中。**

### Axios的特性

- **从浏览器中创建 XMLHttpRequests**
- **支持 Promise API**
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 从 node.js 创建 http 请求
- 客户端支持防御 XSRF

### GET和POST实例

**执行 GET 请求:**
```js
// 为给定 ID 的 user 创建请求
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// 上面的请求也可以这样做
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```
<br>

**执行 POST 请求:**
```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```
<br>

**执行多个并发请求:**
```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
```

## 后记

说句实话,真的是有点心力憔悴.从之前的HTML/CSS/JS到这,感觉就像突然难度上了3个等级一样,可以说每一步都艰难无比.
    
本想今天(2019年11月17日)给自己放个假休息一下的,但是真的害怕像平常寒暑假一样轻松了就拉不住闸了,所以还是坚持写了这篇Axios.

看来看去,感觉就像一个空中楼阁,之前ajax用的就磕磕绊绊,这里更是不知所措.

幸而昨天在网上找到了几门课程,还算是有得方向继续走下去.这篇笔记就暂且如此吧,等有朝一日看得懂教程了我再补全它.