---
layout: post
title: "CSS学习笔记（四）"
subtitle: ''
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-10-27/a.jpg"
tags:
  - 前端学习笔记
---
### 问题

**①css盒模型是什么？**

**②display的值有哪几种，这几种值区别是什么？**

**③css怎么实现垂直水平居中？你能使用几种方式实现？**

**④页面的常见布局有那几种，分别可以怎么实现？**

### 概述
![](../../../../img/in-post/2019-10-27/c.gif)

- 内边距、边框和外边距都是可选的，默认值是零。
- 外边距可以是负值，而且在很多情况下都要使用负值的外边距。


### CSS 内边距
**可以按照上、右、下、左的顺序分别设置各边的内边距,可以使用不同的单位或百分比值,百分数值是相对于其父元素的 width 计算的.**


### CSS 边框
**定义单边宽度**
```
p {border-style: solid; border-width: 15px 5px 15px 5px;}
```
- *注意：由于 border-style 的默认值是 none，如果没有声明样式，就相当于 border-style: none。因此，如果您希望边框出现，就必须声明一个边框样式。*
- *CSS2 引入了边框颜色值 transparent。这个值用于创建有宽度的不可见边框。请看下面的例子：*

### CSS 外边距
格式类似边框写法：
```css
p {margin: 20px 30px 30px 20px;}
```
