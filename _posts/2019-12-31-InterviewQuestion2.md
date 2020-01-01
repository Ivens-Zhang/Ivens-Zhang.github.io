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

### 样式引用优先级

1. 行内样式
2. 内部样式
3. 外部引用样式

*注意: `!important` 可以直接大幅增加优先级 ( 不到万不得已不要用 )*

## 二. 居中
### 水平居中 (4 种方法)

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191231215445.png)


### 垂直居中

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191231232249.png)

通过display:table-cell
```
display: table-cell;
vertical-align: middle;
text-align: center;        
```

使用display:flex实现
```
flex布局会让容器内的元素得到垂直水平居中
display: flex;
justify-content: center;
align-items: center;
```

使用display:-webkit-box实现
```
display: -webkit-box;
-webkit-box-align: center;
-webkit-box-pack: center;
```

绝对定位和0
```
margin: auto;
position: absolute;
top: 0; left: 0; bottom: 0; right: 0; 
```

使用transform:translate定位
```
position: absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
text-align: center;
```

文本垂直水平居中
```
height: 100px;
line-height: 100px;
```

*参考文章 : [《 CSS实现垂直居中的几种方法 》](https://www.cnblogs.com/jing-tian/p/10969887.html)*


## 三. 定位



## 四. 响应式设计
