---
layout: post
title: "CSS学习笔记（三）"
subtitle: ''
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-10-27/a.jpg"
tags:
  - 前端学习笔记
---

### 1.6 CSS 列表

使用[list-style-type][1]和[ list-style-image][2]改变列表标志。

也可以将以上列表样式属性合并为一个方便的属性：[list-style][3].

### 一、CSS 轮廓

![](../../../img/in-post/2019-10-27/a.png)

##### Outline

**outline （轮廓）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。**

可以按顺序设置如下属性：
- outline-color
- outline-style
- outline-width

> 在CSS中注释写法为：/* */

<center><strong>demo——在元素周围画线</strong></center>
![](../../../../img/in-post/2019-10-27/b.png)

**代码如下**
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <style type="text/css">
        p {
            border: red solid 2px;
            /* outline-color、outline-style、outline-width */
            outline: #00ffff dotted 10px;
        }
    </style>
    <title>Document</title>
</head>

<body>
    <p>Internet Explorer 8 （以及更高版本） 才支持 outline 属性。</p>

</body>

</html>
```

### 1.7CSS表格
**text-align 属性设置水平对齐方式，比如左对齐、右对齐或者居中**

**vertical-align 属性设置垂直对齐方式，比如顶部对齐、底部对齐或居中对齐**

**为 td 和 th 元素设置 padding 属性，控制表格中内容与边框的距离**

**background-color控制表格背景色。**

***
<center><strong>小demo——好看的表格</strong></center>

![](../../../../img/in-post/2019-10-26/a.png)


**代码如下：**
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <style type="text/css">
        #customers {
            font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
            width: 100%;
            border-collapse: collapse;
        }

        #customers td,
        #customers th {
            font-size: 1em;
            border: 1px solid #98bf21;
            padding: 3px 7px 2px 7px;
        }

        #customers th {
            font-size: 1.1em;
            text-align: left;
            padding-top: 5px;
            padding-bottom: 4px;
            background-color: #a7c942;
            color: #ffffff;
        }

        .alt {
            color: #000000;
            background-color: #eaf2d3;
        }
    </style>
    <title>Document</title>
</head>

<body>
    <table id="customers">
        <tr>
            <th>Company</th>
            <th>Contact</th>
            <th>Country</th>
        </tr>

        <tr>
            <td>Apple</td>
            <td>Steven Jobs</td>
            <td>USA</td>
        </tr>

        <tr class="alt">
            <td>Baidu</td>
            <td>Li YanHong</td>
            <td>China</td>
        </tr>

        <tr>
            <td>Google</td>
            <td>Larry Page</td>
            <td>USA</td>
        </tr>

        <tr class="alt">
            <td>Lenovo</td>
            <td>Liu Chuanzhi</td>
            <td>China</td>
        </tr>

        <tr>
            <td>Microsoft</td>
            <td>Bill Gates</td>
            <td>USA</td>
        </tr>

        <tr class="alt">
            <td>Nokia</td>
            <td>Stephen Elop</td>
            <td>Finland</td>
        </tr>

    </table>
</body>

</html>
```


[1]:https://www.w3school.com.cn/cssref/pr_list-style-type.asp
[2]:https://www.w3school.com.cn/cssref/pr_list-style-image.asp
[3]:https://www.w3school.com.cn/cssref/pr_list-style-position.asp