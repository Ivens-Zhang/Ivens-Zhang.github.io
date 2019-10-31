---
layout: post
title: "HTML学习笔记"
subtitle: ''
author: "Ivens"
header-mask: 0.2
header-img: "img/in-post/2019-10-24/th.jpg"
hidden: true
tags:
  - 前端学习笔记
---
### 一、带着问题学习

##### 1.html是什么

答：HTML是用来描述网页的一种语言。
- HTML指的是[超文本][1]标记语言（Hyper Text Markup Language）
- 不是编程语言，是标记语言
- 标记语言是一套标记标签
- HTML使用标记标签来描述网页

##### 2.html的基本结构长什么样子
```html
<!DOCTYPE HTML>
<html>
  <body>

  </body>
</html>
```

##### 3.标签是什么
**如：```<input> <p> <h1>``` 这类**

##### 4.属性是什么
**在标签中，提供了有关 HTML 元素的更多的信息。**

<hr>

### 二、知识点

##### ①HTML 标签
- HTML 文档也被称为网页,HTML 文档包含 HTML 标签和纯文本
- 标签对中的第一个标签是开始标签，第二个标签是结束标签,也被称为开放标签和闭合标签
- 指的是从开始标签（start tag）到结束标签（end tag）的所有代码。


##### ②常用标签

###### 1.```<p> </p>```定义段落。
   
###### 2.```<a> </a>```标签

target 属性---你可以定义被链接的文档在何处显示。
> _blank	在新窗口中打开被链接文档。<br>
 _self	默认。在相同的框架中打开被链接文档。<br>
 _parent	在父框架集中打开被链接文档。<br>
 > _top	在整个窗口中打开被链接文档。

###### 3.```<img />``` 插入图片
**替换文本属性（Alt）**<br>
  alt 属性用来为图像定义一串预备的可替换的文本。替换文本属性的值是用户定义的。<br>
```<img src="boat.gif" alt="Big Boat">```<br>
在浏览器无法载入图像时，替换文本属性告诉读者她们失去的信息。此时，浏览器将显示这个替代性的文本而不是图像。为页面上的图像都加上替换文本属性是个好习惯，这样有助于更好的显示信息，并且对于那些使用纯文本浏览器的人来说是非常有用的。

**创建图像映射——本例显示如何创建带有可供点击区域的图像地图。其中的每个区域都是一个超级链接。https://www.w3school.com.cn/tiy/t.asp?f=html_areamap**

###### 4.将图像作为链接
- ```<a href="a.html"><img border="0" src="/b.gif" /></a> ```

###### 5.有序数列无序数列
无序列表始于 `<ul>` 标签。每个列表项始于`<li>`。

有序列表始于 `<ol>` 标签。每个列表项始于 `<li>` 标签。

自定义列表以 `<dl>` 标签开始。每个自定义列表项以 `<dt>` 开始。每个自定义列表项的定义以 `<dd>` 开始。
```html
<dl>
<dt>Coffee</dt>
<dd>Black hot drink</dd>
</dl>
```
**列表项内部可以使用段落、换行符、图片、链接以及其他列表等等。**

*注意：HTML 标签对大小写不敏感：```<P>``` 等同于 ```<p>```。许多网站都使用大写的 HTML 标签，而在未来 (X)HTML 版本中强制使用小写。*

##### ③HTML 元素

没有内容的 HTML 元素被称为空元素。空元素是在开始标签中关闭的,比如 <br />，是关闭空元素的正确方法，HTML、XHTML 和 XML 都接受这种方式。

*注意：当显示页面时，浏览器会移除源代码中多余的空格和空行。所有连续的空格或空行都会被算作一个空格。需要注意的是，HTML 代码中的所有连续的空行（换行）也被显示为一个空格。*

##### ④表单

> **表单用于搜集不同类型的用户输入。**

**1.`<input type="radio">` 定义单选按钮与`<input type="checkbox">` 定义复选框。相同**<br>
name=""属性要一样！ 

**2.`<input type="submit">` 定义用于向表单处理程序（form-handler）提交表单的按钮。**

表单处理程序在表单的 action 属性中指定，如果省略 action 属性，则 action 会被设置为当前页面。

method 属性规定在提交表单时所用的 HTTP 方法（GET 或 POST）

>何时使用 GET？
您能够使用 GET（默认方法）：
如果表单提交是被动的（比如搜索引擎查询），并且没有敏感信息。当您使用 GET 时，表单数据在页面地址栏中是可见的
注释：GET 最适合少量数据的提交。浏览器会设定容量限制。

>何时使用 POST？
您应该使用 POST：
如果表单正在更新数据，或者包含敏感信息（例如密码）。POST 的安全性更加，因为在页面地址栏中被提交的数据是不可见的。

**3.用 `<fieldset>` 组合表单数据，https://www.w3school.com.cn/tiy/t.asp?f=html_form_legend**<br>
`<fieldset>` 元素组合表单中的相关数据<br>
`<legend>` 元素为 `<fieldset>` 元素定义标题。

**4.下拉框，通过添加 selected 属性来定义预定义选项**
```html
<select name="cars">
<option value="volvo">Volvo</option>
</select>
```

**5.HTML5 `<datalist>` 元素，https://www.w3school.com.cn/tiy/t.asp?f=html_elements_datalist**

`<datalist>` 元素为 `<input> `元素规定预定义选项列表。
用户会在他们输入数据时看到预定义选项的下拉列表。
`<input>` 元素的 list 属性必须引用 `<datalist>` 元素的 id 属性。

**6.常见表单输入类型：https://www.w3school.com.cn/html/html_form_input_types.asp**

- value 属性规定输入字段的初始值
- readonly 属性规定输入字段为只读（不能修改）
- disabled 属性规定输入字段是禁用的，是不可用和不可点击的，不会被提交。
- size 属性规定输入字段的尺寸（以字符计）
- maxlength 属性规定输入字段允许的最大长度

[1]:https://www.w3school.com.cn/tags/tag_term_hypertext.asp