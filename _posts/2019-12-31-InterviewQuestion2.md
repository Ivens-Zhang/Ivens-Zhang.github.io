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

**本文按下图思路, 对 2020 春招面试 CSS 部分知识点进行总结.**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200103215703.png)

## 前言
**`HTML` 的元素分类**

- **按默认样式分**

| 按默认样式分 | 常见元素                   |
| ------------ | -------------------------- |
| 块级元素 `block`   | **区块元素 >** `div / section`     |
| 行内元素 `inline`  | **文本相关标签 >** `span / strong` |
| 行内块级元素 <br> `inline-block` | **表单元素 >** `input / selection` |

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

*注意: 使用 `!important` 可以直接大幅增加优先级 ( 不到万不得已不要用 )*

## 二. 居中
### 水平居中 (4 种方法)

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191231215445.png)


### 垂直居中 (6 种方法)

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


## 三. 定位 ★
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

适配小屏布局思路:
- **`隐藏 + 折行 + 自适应空间`**

响应式布局设计流程:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200101235353.png)

---

### `em` 与 `rem` 的区别?
`em` 的值并不是固定的，它会继承父级元素的字体大小。如果当前对行内文本的字体尺寸未被设置，则相对于浏览器的默认字体尺寸。

 `rem` 相对大小相对的只是HTML根元素, 不会受到父级的影响。目前，除了 `IE8` 及更早版本外，所有浏览器均已支持rem。
 
 **注意: 在搭配 `media 媒体查询`使用 `rem` 时, 记得给 `html` 选择器的 `font-size` 属性设置一个绝对大小的值.**

## 五. CSS3 新特性 ★

### `transition`
`transition` 补间动画, 是通过 CSS3 计算生成的动画效果.


**过渡属性:**
![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200102140727.png)

最简单的 Demo:
```html
<head>
  <style>
    .container{
      width: 100px;
      height: 100px;
      background: red;
      transition: all 1s;   /* 也可以只规定 width 具体属性进行渐变 */
    }
    .container:hover{
      width: 300px;
    }
  </style>
</head>
<body>
  <div class="container"></div>
</body>
```
![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/2020-1-3252124235.gif)

在 `transition` 中不光宽度高度可以计算补间动画, 颜色也可以.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/2020-1-325234123.gif)

*注 : transition-timing-function 属性可以在这个网站进行自定义 --- [`Ceaser CSS EASING ANIMATION TOOL`](https://matthewlein.com/tools/ceaser)*

---

### `animate`

关键帧动画
- 相当于多个补间动画
- 定义更灵活

```
animation: name duration timing-function delay iteration-count direction fill-mode;
```

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200102150249.png)

*注 : animation-fill-mode 属性规定动画在播放之前或之后，其动画效果是否可见。*

参考文章：
- [《 CSS3 animation 属性参考手册 》](https://www.w3school.com.cn/cssref/pr_animation.asp)
- [《 CSS3 animation-fill-mode 属性参考手册 》](https://www.w3school.com.cn/cssref/pr_animation-fill-mode.asp)

---

补充：逐帧动画
- 用于无法补间计算的动画
- 结合雪碧图使用, 避免文件过大
- 使用steps(), 取消各帧之间的补间动画

如果不使用 `steps()` 会怎样?

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/754767-20160601114839946-1209231470.gif)

原因是: `animation` 默认以 `ease` 方式过渡，它会在每个关键帧之间插入补间动画，所以动画效果是连贯性的.

解决方法: 使用 `steps()` 帧之间的阶跃动画. 下图为 `steps` 的原理:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200102152220.png)

由上图可知:
- steps(1,start): 动画一开始就跳到 100% 直到下一帧
- steps(1,end): 保持 0% 的样式直到下一帧.

*用一个成语来形容: `steps(1,end)` 就是寅吃卯粮, 拖到最后才变化.*

附 `animation` 思维导图一张:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/754767-20160531142159774-1968341984.jpg)


*参考文章 : [《 css3 animation实现逐帧动画 》](https://www.cnblogs.com/Fengzp/p/5548493.html)*

---

### `transform`
使用语法:
```css
transform: translate | scale | skew | rotate ;
```

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200102171747.png)

- **2D 转换**

**`translate()` 方法，元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数**, 如：
- `transform: translate(50px,100px); 把元素从左侧移动 50 像素，从顶端移动 100 像素.`

**`scale()` 方法，元素的尺寸会增加或减少，根据给定的宽度（X 轴）和高度（Y 轴）参数**, 如：
- `transform: scale(2,4); 宽度转换为原始尺寸的 2 倍，把高度转换为原始高度的 4 倍. 也可以只写一个参数, 即宽高进行等比例缩放. `

**`skew()` 方法，元素翻转给定的角度，根据给定的水平线（X 轴）和垂直线（Y 轴）参数**, 如：
- `transform: skew(30deg,10deg); 围绕 X 轴把元素翻转 30 度，围绕 Y 轴翻转 10 度.`

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200102180456.png)

**`rotate()` 方法，元素顺时针旋转给定的角度。允许负值，元素将逆时针旋转。**如: 
- `transform: rotate(30deg); 把元素顺时针旋转 30 度.`

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200102180613.png)

---

- **3D 转换**

实现 3D 转换的关键在于` x 轴、y 轴、z 轴` 的变换.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200102182053.png)

简单Demo:
```html
<!DOCTYPE html>
<head>
  <style>
    .container{
      height: 400px;
      width: 400px;
      border: 1px solid red;
    }
    .one{
      height: 200px;
      width: 200px;
      background-color: pink;
      transition: all 1s;
    }
    .one:hover{
      transform: skew(20deg,20deg);
      border-radius: 50%;
      background-color: rebeccapurple;
      transform: rotate(45deg) scale(.5);
      transform: rotateY(180deg) rotateX(180deg) rotateZ(180deg);
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="one"></div>
  </div>
</body>
</html>
```
效果如图:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/202013221551.gif)

---

## 六. 预处理器
### 相较原生 `CSS` 的优势
- 添加了很多 CSS 不具备的特性
- 提升了 CSS 文件的组织
- 使 CSS 更易维护和扩展 
- 增加了一些编程特性，无需考虑浏览器的兼容性问题

三大主流 CSS 预处理器:

**`Sass：`**

Sass 是采用 Ruby 语言编写的一款 CSS 预处理语言，它诞生于2007年，是最大的成熟的 CSS 预处理语言。最初它是为了配合 HAML（一种缩进式 HTML 预编译器）而设计的，因此有着和 HTML 一样的缩进式风格.

虽然缩进式风格可以有效缩减代码量，强制规范编码风格，但它一方面并不为大多数程序接受，另一方面无法兼容已有的 CSS 代码。这也是 Sass 虽然出现得最早，但远不如 LESS 普及的原因.

Sass 和 SCSS 其实是同一种东西，我们平时都称之为 Sass，两者之间不同之处有以下两点：
1. 文件扩展名不同
2. Sass 是以严格的缩进式语法规则来书写，不带大括号{}和分号; 而 Scss 的语法书写类似于 CSS. 

**`Less：`**

Less 是一门采用 JavaScript 语言编写 CSS 预处理语言.

它扩充了 CSS 语言，增加了诸如变量、混合（mixin）、函数等功能，让 CSS 更易维护、方便制作主题、扩充。

Less 可以运行在 Node 或浏览器端.

**`Stylus:`**

可以省略原生CSS中的大括号{}，逗号， 和分号 ；类似于python语言的编程风格

由于其语法灵活的问题，如果没有团队规范，那么就会带来团队开发混乱，维护起来比较麻烦，各种语法混杂。

---

**综上, 我决定以 `Less` 语言作为主力预处理器, 原因在于: 语法简单, 适用性广, 更为普及.**

*可能有人会觉得 Less 在处理一些复杂语法时不如 Sass, 但我想我自己并非专业 CSS 工程师, 也不会在 CSS 中运用到一些极度复杂的语法.*


### 预处理器新特性
- 嵌套反映层级和约束

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/202013105147.jpg)

Less 提供了使用 `嵌套（nesting）`代替层叠或与层叠结合使用的能力。用 Less 书写的代码更加简洁，并且模仿了 HTML 的组织结构( `&` 表示当前选择器的父级 )。

- 变量

无需多说，看代码一目了然：
```less
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```
编译为：
```css
#header {
  width: 10px;
  height: 20px;
}
```

- `Mixin` 和 `Extend`

`混合（Mixin）`是一种将一组属性从一个规则集包含（或混入）到另一个规则集的方法。假设我们定义了一个类（class）如下：
```less
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

如果我们希望在其它规则集中使用这些属性呢？没问题，我们只需像下面这样输入所需属性的类（class）名称即可，如下所示：
```less
#menu a {
  color: #111;
  .bordered();
}

.post a {
  color: red;
  .bordered();
}
```
编译结果:
```css
#menu a {
  color: #111;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

.post a {
  color: red;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

`.bordered` 类所包含的属性就将同时出现在 `#menu a` 和 `.post a` 中了。

关于 `Mixins` 还有几个知识点:

i. `.class 选择器` 和 `#id 选择器` 都可以作为 mixins 使用

ii. `mixins` 定义时如果加上 `()` 自身则不会被编译出来.

iii. `mixins` 不仅可以包含属性，还可以包含选择器.

For example:
```less
.my-hover-mixin() {
  &:hover {
    border: 1px solid red;
  }
}
button {
  .my-hover-mixin();
}
```
Outputs:
```css
button:hover {
  border: 1px solid red;
}
```
---

`拓展 (Extend)` 是一个LESS伪类，它通过使用 `:extend` 选择器在一个选择器中扩展其他选择器样式。

**既然 `extend` 和 `mixins` 都可以用作引用其他类中的样式, 那么他们在使用的场景上有什么差异呢?**


一个标准按钮样式：
```less
.btn {
  background: blue;
  color: white;
}
```

如果你想在一个更具体的按钮中引用该样式, 可以使用 mixins 轻松地将该类混合到另一个类中，从而避免逐个进行复制粘贴：

```less
.round-button {
  .btn;
  border-radius: 4px;
}
```

乍一看，这似乎没有问题，但是如果查看编译后的 CSS 文件，我们会发现重复项浪费了我们很多空间:

```css
.btn {
  background: blue;
  color: white;
}

.round-button {
  background: blue;
  color: white;
  border-radius: 4px;
}
```

在 CSS 中我们可以对选择器进行分组，用较少的代码量来完成复用：

```css
.btn, .round-button {
  background: blue;
  color: white;
}

.round-button {
  border-radius: 4px;
}
```

那在 Less 中可以完成相同工作是什么呢? 没错, 那就是 `Extend`, 它发布与 Less @1.4 版本中.

```less
.btn {
  background: blue;
  color: white;
}

.round-button {
  &:extend(.btn);
  border-radius: 4px;
}
```

编译后结果：

```css
.btn, .round-button {
  background: blue;
  color: white;
}

.round-button {
  border-radius: 4px;
}
```

注意:
- *extend 使用类中样式, 默认不引用其中子选择器. 可以通过添加 all 关键字来强制编译器包括所有嵌套的选择器.*

```less
.footer {
  padding: 20px;
  h2 {
    color: white;
  }
}

.feed {
  &:extend(.footer all);
}
```

编译输出：

```css
.footer, .feed {
  padding: 20px;
}

.footer h2, feed h2 {
  color: white;
}
```

还可以指定特定的嵌套选择器：

```less
.feed {
  &:extend(.footer h2);
}
```

- *extend 只会在同一媒体查询中扩展选择器。*

√ 有效：
```less
@media (max-width: 1024px) {
  .feed {
    padding: 20px;
  }
  .sig {
    &:extend(.feed);
  }
}
```

× 无效:
```less
.feed {
  padding: 20px;
}

@media (max-width: 1024px) {
  .sig {
    &:extend(.feed);
  }
}
```

所以, 说下结论:

**使用 `extend` 比使用 `mixins` 最后编译生成的代码大小会小一些.**

**只复用静态样式时使用 `extend`, 需要传参动态复用样式时使用 `mixins`.**

```less
.btn(@color) {
  background: @color;
  border: 1px solid darken(@color, 15);
  color: #fff;
  &:hover {
    background: lighten(@color, 10);
    border-color: darken(@color, 5);
  }
}
```

这种情况下使用 `mixin`，可以非常轻松地定义一堆不同的按钮：

```less
.btn-success {
  .btn(#18682f);
}

.btn-alert {
  .btn(#9b6910);
}

.btn-error {
  .btn(#9b261a);
}
```

---

- 循环

`Loops` 语句允许我们多次执行一个语句或一组语句。 当`递归 mixin` 与 `Guard 表达式`和模式匹配组合时，可以创建各种迭代/循环结构。

语法:

```css
.循环名(参数1, 参数2, ..) when (条件1) and (条件2){
  // 真正被编译出的部分
  ...
  // 用来进入下一个循环部分
  .循环名(参数运算, 如: @n+1,..);
}
```

实例:
```less 
.loop(@n,@i) when(@n<3) and (@i<5) {
  #@{n}{
    width: 1000 * @i px;
  }
  .col-@{n}{
    font-size: 10 * @n px;
  }
  .loop(@n+1,@i+1);
}

div{
  .loop(1,3);
}
```
编译后结果:

```css
div #1 {
  width: 3000 px;
}
div .col-1 {
  font-size: 10 px;
}
div #2 {
  width: 4000 px;
}
div .col-2 {
  font-size: 20 px;
}
```

*循环中可以包括 `#id 选择器`, `.class 选择器`, `也可以不加选择器直接写属性`, 不过在使用循环时要放在选择器中, 不然会编译失败.*

---

- `@import CSS` 文件模块化

“导入”的工作方式如预期的一样。你可以导入一个 `.less` 文件，此文件中的所有变量就可以全部使用了：

我们可以将一个复杂页面的样式拆分为几个部分, 如: `header` / `footer` / `variable` 等. 

一个 less 文件负责对应一块的样式, 最后编译各个 less 文件导入的总文件即可.

如果导入的文件是 `.less` 扩展名，则可以将扩展名省略掉:
```less
@import "library"; // library.less
@import "typo.css";
```

*参考文档 ：[《 Less 中文网 》](https://less.bootcss.com/)*

