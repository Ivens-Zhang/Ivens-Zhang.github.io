---
layout: post
title: "面向 2020 春招面试题(六)"
subtitle: "———— 高频面试题部分"
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-12-4/th.jpg"
hidden: true
tags:
  - 春招面试题
---

## 几乎必问的问题

webpack 的原理 ★

XSS 和 CSRF 的防御 ★

实现垂直水平居中★

url 页面请求的过程 (输入网址后的每一步) ★

Vue 双向数据绑定及原理 ★

跨域 ★

http 状态码 ★

防抖与节流(手写防抖)★

CSS 实现三列布局 / 两列布局 ★

页面性能优化 ★



## HTML 相关问题

### html5 新增特性

- 语义标签

  - 使开发者更方便清晰构建页面的布局

- 增强型表单

- 视频和音频

  - 音频：`<audio src=" "></audio>`
  - 视频：`<video src=" "></video>`

- `Canvas` 绘图与 `SVG` 绘图

  - SVG与Canvas区别

    *SVG适用于描述XML中的2D图形的语言

    *Canvas随时随地绘制2D图形（使用javaScript）

    *SVG是基于XML的，意味这可以操作DOM，渲染速度较慢

    *在SVG中每个形状都被当做是一个对象，如果SVG发生改变，页面就会发生重绘

    *Canvas是一像素一像素地渲染，如果改变某一个位置，整个画布会重绘。

    | Canvas                           | SVG                        |
    | -------------------------------- | -------------------------- |
    | 依赖分辨率                       | 不依赖分辨率               |
    | 不支持事件处理器                 | 支持事件处理器             |
    | 能够以.png或.jpg格式保存结果图像 | 复杂度会减慢搞渲染速度     |
    | 文字呈现功能比较简单             | 适合大型渲染区域的应用程序 |
    | 最合适图像密集的游戏             | 不适合游戏应用             |

- 地理定位

  - 使用`getCurrentPosition()`方法来获取用户的位置。以实现“LBS服务”

- 拖放 API

  - `draggable` 属性

- WebWorker

  - 通过加载一个脚本文件，进而创建一个独立工作的线程，在主线程之外运行.

- WebStorage

  - localStorage
  - sessionStore

- WebSocket

详细内容参考: [《HTML5 新特性总结》](https://www.cnblogs.com/binguo666/p/10928907.html)

---

HTML5 元素分类:

- 按默认样式分

| 按默认样式分          | 常见元素                                                   |
| --------------------- | ---------------------------------------------------------- |
| 块级 block            | 区块元素, 如: `div、p、ul、ol、li、form、h1~h6、table`     |
| 行内 inline           | 文本相关标签, 如: `span、a、strong、em、i、del、s、ins、u` |
| 行内块级 inline-block | 表单元素, 如:  `input、 img、td`                           |

- 按内容分

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191222160615.png" style="zoom:67%;" />

**`Metadata`** <br>
顾名思义，Metadata元素意指那些定义文档元数据信息的元素 — 其作用包括：影响文档中其它节点的展现与行为、定义文档与其它外部资源之间的关系等。以下元素属于 `Metadata：base, link, meta, noscript, script, style, template, title`

**`Flow`** <br>
所有可以放在body标签内，构成文档内容的元素均属于Flow元素。

**`Sectioning`** <br>
指定义页面结构的元素，具体包含以下四个：`article, aside, nav, section`

**`Heading`** <br>
所有标题元素属于 Heading，也即以下6个元素：`h1, h2, h3, h4, h5, h6`

**`Phrasing`** <br>
所有可以放在p标签内，构成段落内容的元素均属于 Phrasing 元素。比如：`em、strong` ，另外如：`audio、video、img、select、input` 等元素(经测试，这些元素都可以放置在p标签中)。Phrasing 元素内部一般只能包含别的 Phrasing 元素。


**`Embedded`** <br>
所有用于在网页中嵌入外部资源的元素均属于Embedded元素，具体包含以下9个：`audio, video, img, canvas, svg, iframe, embed, object, math`

**`Interactive`** <br>
所有与用户交互有关的元素包括 `a, input, textarea, select` 等。

细节参考:  [《W3C HTML5标准阅读笔记 – 元素分类》](https://www.zhangjinglin.cn/blog/d37dbcd34db6df9e7adfbe3ae2d8ad95.html)

***

**HTML 元素的嵌套关系**

- 块级元素可以包含行内元素
- 块级元素不一定能包含块级元素
- 行内元素一般不能包含块级元素
  - 反例: `a` 可以包含 `div`

---

**HTML 的默认样式**

- 每个浏览器都会自带默认样式, 但是在开发中会造成页面不统一的问题.
- 解决方案: CSS Reset
  - `CSS Tool: Reset CSS` & `YUI: CSS Reset` --- 清除浏览器默认样式 
  - `Normalize.css` --- 统一各浏览器默认样式
  - *{margin:0;padding:0} --- 最简单方法, 可能存在性能浪费

---

**真题:**

`DOCTYPE` 的意义?

- 让浏览器以标准模式渲染, 如: 盒子模型宽度是否包含 padding 值.
- 让浏览器知道元素的合法性, 如: 在 XHTML 中标签必须完全闭合.

**HTML / XHTML / HTML5 的关系**

- HTML 属于 SGML
- XHTML 是对 HTML 进行 XML 严格化的结果
- HTML5 不属于 SGML 或 XML, 是一种独立的规范

**em 和 i 的区别**

- `em` 表示强调, `i` 纯样式标签表示斜体
- 在 HTML5 中 `i` 一般用作图标

**语义化的意义**

- 代码可读性更强, 容易维护
- 浏览器容易理解结构
- 有助于 SEO

**哪些元素可以自闭合**

- 表单元素: input
- img
- br / hr
- head 中的 meta / link

**HTML 与 DOM 的关系**

- DOM 是由 HTML 解析而来的
- JavaScript 维护的是 DOM 而不是 HTML






http 缓存机制 / 协议 / 优化

webSocket 基础理解

http 2.0 与 1.0 的区别

http 和 https 的区别

## CSS 相关问题

圣杯布局 / 双飞翼布局

CSS 媒体查询

position 取值与区别

清除浮动

transition 和 animation

CSS 里的 flex

display 的取值和区别

几种图片格式的区别

### 一像素边框

- 解决思路 : `伪类 + transform`

```
li{position: relative;}
li:after{
  position: absolute;
  bottom:0;
  left:0;
  content: '';
  width:100%;
  height:1px;
  border-top:1px solid black;
  transform: scaleY(0.5);//注意兼容性
}
```

这种方式的原理很简单，就是把原先元素的 border 去掉，然后利用 `:before` 或者 `:after` 重做 `border` ，并 `transform` 的 `scale` 缩小一半，原先的元素相对定位，新做的 border 绝对定位。个人认为这是比较完美的做法。

## JavaScript 相关问题

继承的理解

new 的思路

### 正则

[《 JS正则表达式入门，看这篇就够了 》](https://segmentfault.com/a/1190000009324194)

typeof 和 instanceOf 的区别

手写一个 promise

图片懒加载

diff (可能手写)

事件循环

ES6 的 async 和 await 和 promise

深拷贝和浅拷贝

全局变量污染的解决

快速排序和二分查找



## Vue 相关问题

Vue 子父组件之间的通信

mvc 和 mvp 和 mvvm

babel 原理 (简单了解)



## 计算机网络相关问题

三次握手, 四次挥手

tcp 和 udp






