---
layout: post
title: "面向 2020 春招面试题(二)"
subtitle: "CSS 部分考点"
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-12-4/th.jpg"
hidden: true
catalog: true
tags:
  - 春招面试题
---

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191231183057.png)

**本文按上图思路, 对 CSS 部分知识点进行总结.**

## 前言
**`HTML` 的元素分类**

- **按默认样式分**

| 按默认样式分 | 常见元素                   |
| ------------ | -------------------------- |
| 块级元素 `block`   | **区块元素** `div / section`     |
| 行内元素 `inline`  | **文本相关标签** `span / strong` |
| 行内块级元素 <br> `inline-block` | **表单元素** `input / selection` |

`block` 元素和 `inline-block` 元素可以自己定义宽高, 而 `inline` 不可以. <br>
`inline` 和 `inline-block` 可以与其他元素并列在同一行, 而 `block` 不可以.

- **按内容分**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191222160615.png)

## 一. 优先级

### 选择器优先级
`ID 选择器 (100)` **>** `类 / 属性 / 伪类(:hover{}) (10)` **>** `元素 / 伪元素(::before{}) (1)` **>** `其他选择器 (0)`

例子:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191222224200.png)

*P.S. 低权重选择器叠加再多也不如一个高权重选择器优先, 上文的权重数值只在其他条件一致时有用.*

