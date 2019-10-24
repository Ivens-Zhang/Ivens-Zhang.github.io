---
layout: post
title: "CSS学习笔记"
subtitle: ''
author: "Ivens"
header-mask: 0.2
header-img: "img/in-post/2019-10-25/th.jpg"
tags:
  - 前端学习笔记
---
### 一、带着问题学习

**①css是什么？**

CSS 指层叠样式表 (Cascading Style Sheets)，样式定义如何显示 HTML 元素。

> 由于两种主要的浏览器（Netscape 和 Internet Explorer）不断地将新的 HTML 标签和属性（比如字体标签和颜色属性）添加到 HTML 规范中，创建文档内容清晰地独立于文档表现层的站点变得越来越困难。<br>
为了解决这个问题，万维网联盟（W3C），这个非营利的标准化联盟，肩负起了 HTML 标准化的使命，并在 HTML 4.0 之外创造出样式（Style）。

**②css的语法是怎么样的结构？**

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明。

```selector {declaration1; declaration2; ... declarationN }```

**③怎么对一个标签增加简单的样式，自己尝试一下？**

```h1 {color:red; font-size:14px;}```
<br>*提示：请使用花括号来包围声明*

**④css盒模型是什么？**

**⑤display的值有哪几种，这几种值区别是什么？**

**⑥css怎么实现垂直水平居中？你能使用几种方式实现？**

**⑦页面的常见布局有那几种，分别可以怎么实现？**

### 二、知识点

#### 1.CSS 基础

###### 1.1颜色值的不同写法和单位

> ```p { color: #ff0000; }```

> ```p { color: #f00; }```

> ```p { color: rgb(255,0,0); }```

> ```p { color: rgb(100%,0%,0%); }```

###### 1.2选择器的分组
用逗号将需要分组的选择器分开。
```css
h1,h2,h3,h4,h5,h6 {
  color: green;
  }
```
###### 1.3继承及其问题

通过 CSS 继承，子元素将继承最高级元素（在本例中是 body）所拥有的属性（这些子元素诸如 p, td, ul, ol, ul, li, dl, dt,和 dd）。

*创建一个针对子元素的特殊规则，这样它就会摆脱父元素的规则*

###### 注意事项

- 如果值为若干单词，则要给值加引号
- 用分号将每个声明分开
- 如果涉及到与 HTML 文档一起工作的话，class 和 id 名称对大小写是敏感的。
