---
layout: post
title: "面向 2020 春招面试题(二)"
subtitle: "————CSS 部分考点"
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
有时, 我们希望规定元素应该生成的框的类型, 可以使用 `display` 属性进行控制.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200101104700.png)

*其他 `display` 属性参考 : [《 CSS display 属性 》](https://www.w3school.com.cn/css/pr_class_display.asp)*



### CSS position 属性

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191224144150.png)

利用 `position` 属性, 可以对页面进行布局, 如: 

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200101113345.png)

*参考文章 :*
- [《 CSS Grid 网格布局教程 》](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)
- [《 三栏布局的5种解决方案及优缺点 》](https://www.cnblogs.com/chengzp/p/layout.html)

### 重点 ——— 清除浮动
提及 `float` 布局时, 有一个我们避不开的话题, 那就是**清除浮动**, 而清除浮动的源头则是 `float` 布局带来的高度塌陷问题.

1.什么是高度塌陷?

```
<div class="container">
  <div class="left"></div>
  <span>aaaaaa</span>
  <div class="right"></div>
</div>
```
父元素包含了两个 `float` 元素, 一个 `inline` 元素. 给父元素的边框显示高度. 可以看出: 
- **父元素高度并没有被 `float` 撑开**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200101115226.png)

 如果该父元素后出现其他元素, 页面就会变成这样:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200101115251.png)

**float 元素并没有把父元素高度撑起来, 这种情况就叫做高度塌陷. 而清除浮动就是解决高度塌陷所带来的问题.**

那么, 如何清除浮动呢? 方法如下:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200101210356.png)

*参考文章 : [《 CSS清除浮动常用方法小结 》](https://www.cnblogs.com/theWayToAce/p/5536781.html)*

## 四. 响应式设计

**响应式设计**可以根据屏幕分辨率自适应以及自动缩放图片、自动调整布局，这不只是技术的实现，更多的是对于设计的全新思维模式。

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/a.jpg)

上图是我博客的主页, 左图是屏幕宽度大于 800px 时的布局, 右图是小于 800px 时的布局.

---

更改布局思路:
- **`隐藏 + 折行 + 自适应空间`**

响应式布局设计流程:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200101235353.png)

---

### `em` 与 `rem` 的区别?
`em` 的值并不是固定的，它会继承父级元素的字体大小。如果当前对行内文本的字体尺寸未被设置，则相对于浏览器的默认字体尺寸。

 `rem` 相对大小相对的只是HTML根元素, 不会受到父级的影响。目前，除了 `IE8` 及更早版本外，所有浏览器均已支持rem。
 
 **注意: 在搭配 `media 媒体查询`使用 `rem` 时, 记得给 html 选择器的 `font-size` 属性设置一个绝对大小的值.**