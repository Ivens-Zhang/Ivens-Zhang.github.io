---
layout: post
title: "<<侧边无延迟菜单>>思路分析及源码"
subtitle: ''
author: "Ivens"
header-mask: 0.2
header-img: "img/in-post/2019-11-3/th.jpg"
tags:
  - 前端学习笔记
---

*11.2日早开始学习课程[<JS实现京东无延迟菜单效果>][1],至今项目一共写了3遍.*

- 第一遍是完全照着老师的视频敲一遍.


- 第二遍是照着自己的敲一遍外加理清思路,中间遇到了一些莫名其妙的问题,花的时间是最长的.


- 第三遍则是加入了一些自己的想法在里面,用时最少.

现在趁热打铁,写一篇思路分析,帮助自己巩固一下.

### 正文

#### 一.项目目标
模仿京东首页制作一个侧边菜单,满足以下要求:
- 将鼠标放在主菜单上会弹出子菜单.
- 用户正常从主菜单进入子菜单鼠标斜向移动时子菜单不会切换.

**如图:**
![](../../../../img/in-post/2019-11-3/a.png)

#### 二.代码思路
①项目结构
- index.html
- mian.js
- mian.css

*其中在index.html中外联引用了百度的JQuery文件.*
```html
 <script src="http://libs.baidu.com/jquery/2.1.4/jquery.min.js"></script>
 ```

 ②实现顺序

1. 先完成html文件,把主菜单和子菜单内容写好.
2. 完成CSS样式
3. 最后写JS代码

③具体步骤











[1]:https://www.imooc.com/learn/829