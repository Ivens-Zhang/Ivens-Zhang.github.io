---
layout: post
title: "CSS学习笔记（二）"
subtitle: ''
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-10-26/th.jpg"
catalog: true
hidden: true
tags:
  - 前端学习笔记
---
###### 1.缩进

```
p {text-indent: 5em;} //可以设置为负值与百分比
```

###### 2.对齐

使用text-align属性。

 **text-align:center 与 `<CENTER>`** 

您可能会认为 text-align:center 与 `<CENTER>` 元素的作用一样，但实际上二者大不相同。<br>
`<CENTER>` 不仅影响文本，还会把整个元素居中。text-align 不会控制元素的对齐，而只影响内部内容。元素本身不会从一段移到另一端，只是其中的文本受影响。

**水平对齐属性是 justify**

在两端对齐文本中，文本行的左右两端都放在父元素的内边界上。然后，调整单词和字母间的间隔，使各行的长度恰好相等。

###### 3.字间隔

word-spacing 属性可以改变字（单词）之间的标准间隔。其默认值 normal 与设置值为 0 是一样的。(**中文无效**)

letter-spacing 属性与 word-spacing 的区别在于，字母间隔修改的是字符或字母之间的间隔。(**中文有效**)

###### 4.大小写转换

text-transform 属性处理文本的大小写。这个属性有 4 个值：
- none
- uppercase
- lowercase
- capitalize

###### 5.文本装饰

text-decoration 有 5 个值：
- none
- underline
- overline
- line-through
- blink

**注意：text-decoration 值会替换而不是累积起来。**
###### 6.处理空白符

white-space 属性

当 white-space 属性设置为 normal 时，会合并所有的空白符，并忽略换行符。

###### 7.CSS字体

通过font-family属性选择字体。
```css
body{font-family : sans-serif;}
```

```css
p {
    font-family: Times,'New Century Schoolbook', serif;
}
```
根据这个列表，用户代理会按顺序查找这些字体，如果都不可用，就会简单地选一种serif字体。

*注意：使用了单引号。只有当字体名中有一个或多个空格（比如 New York），或者如果字体名包括 # 或 $ 之类的符号，才需要在 font-family 声明中加引号。*

###### 8.字体风格
font-style 属性最常用于规定斜体文本，该属性有三个值：
- normal - 文本正常显示
- italic - 文本斜体显示
- oblique - 文本倾斜显示

###### 9.字体粗细

font-weight属性设置文本粗细。
```css
p{
    font-weight:900;
}
```

###### 10.大小

font-size属性。

**W3C 推荐使用 em 尺寸单位。**

###### 11.设置链接的样式

链接的四种状态：

- a:link - 普通的、未被访问的链接
- a:visited - 用户已访问的链接
- a:hover - 鼠标指针位于链接的上方
- a:active - 链接被点击的时刻

**background-color 属性规定链接的背景色**

注意，请按照以下次序规则：

*a:hover 必须位于 a:link 和 a:visited 之后*
*a:active 必须位于 a:hover 之后*
