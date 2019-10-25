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

### 一、CSS 轮廓

![](../../../img/in-post/2019-10-27/a.png)

##### 1.1Outline

**outline （轮廓）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。**

可以按顺序设置如下属性：
- outline-color
- outline-style
- outline-width

<center><strong>demo——在元素周围画线</strong></center>
![](../../../img/in-post/2019-10-27/b.png)

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


