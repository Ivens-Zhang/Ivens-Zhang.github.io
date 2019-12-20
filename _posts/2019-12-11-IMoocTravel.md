---
layout: post
title: "慕课网课程 《去哪儿 APP》 思路分析及源码"
subtitle: '————基于 Vue 2.x 版本'
author: "Ivens"
header-mask: 0.2
catalog: true
header-img: "img/in-post/2019-12-17/th.jpg"
tags:
  - Vue.js
---

`<router-view>` 默认显示 `path: '/'` 的组件. 

`template` 只能对外暴露一个标签.

## 前言

这篇文章为：[《Vue2.5开发去哪儿网App
从零基础入门到实战项目》](https://coding.imooc.com/class/203.html#Anchor) 课程项目实战部分笔记.

第一次使用 `GitHub` + `GitHub Desktop` 完成项目, 了解了如何正确地使用代码版本控制工具. 对 `branch`, `commit`, `merge` 等指令有了初步的理解.

## 正文

### 搭建项目

**首先是在开始搭建项目前的准备工作, 如图安装好对应版本:**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217163927.png)

**其次, 在 `cmd` 中输入 `vue ui` 使用网页端创建 `vue` 项目:**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217164250.png)

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217164329.png)

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217164403.png)

**最后, 打开 `GitHub Desktop`, 左上角菜单栏中 `File >> Create a new repository`.**

到这里, 去哪儿APP项目的初步搭建完成.

### 项目结构
![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217174843.png)

上图是我们刚刚创建好的项目的结构, 说一些我的理解:
- 因为`vue-cli`版本的变化, 不同的版本创建出的项目结构也会不同, 百度一下这个文件夹起什么用很有意义.
- 在我这个版本中没有`config`文件夹, 一些配置文件需要自己在`src`目录下创建`vue.config.js`文件.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217180643.png)


### 初始化项目
按照下图的思路, 为我们的项目新增一些文件和文件夹.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217160336.png)

1. `mock` 文件夹以及三个 json 文件
2. `styles` 文件夹以及两个 css 文件
3. `views` 文件夹中新建三个对应不同组件的文件夹

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217204359.png)

### 添加引用
![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217210312.png)

1.第三方 `css` 库文件添加

在 `mian.js` 中:
```
import './assets/styles/reset.css'
import './assets/styles/border.css'
```

2.添加 `vue` 库文件

2.1. 解决 `300ms 延迟`问题, 在 cmd 中:
```cmd
npm install fastclick --save
```

2.2. 实现图片连播
```cmd
npm install vue-awesome-swiper@2.6.7 --save
```

2.3. 实现更好的屏幕滑动
```cmd
npm install better-scroll --save
```

2.4. http 请求需要的库
```cmd
npm install axios --save
```

3.修改 `index.html`
在 `viewport` 的 `content` 中添加一下代码, 阻止手指对页面的放大缩小:
```
minimum-scale=1.0,maximum-scale=1.0,user-scalable=no
```

***
这样项目中要使用到的第三方库就全部安装导入完成.

### `Home` 组件实现
目标效果:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191218110944.png)

所以我们把 `Home` 页面拆分为以下几个部分, 并分别创建各子组件.

```cmd
F:.
│  Home.vue
│  
└─components
        Header.vue
        Icons.vue
        Recommend.vue
        Swiper.vue
        Weekend.vue
```

在 `Header.vue` 中, 我们要使用 `iconfont` 图标库.

使用方法如下:
1. 进入 `iconfont` 官网, 将图标添加至项目.
2. 进入我的项目, 点击下载到本地.
3. 在项目 `src/ assets/ styles` 中新建文件夹 `iconfont`, 将下载后解压的对应文件放入其中.
4. 将 `iconfont.css` 中路径进行调整.

```cmd
F:.
└─styles
    │  border.css
    │  iconfont.css
    │  reset.css
    │
    └─iconfont
            iconfont.eot
            iconfont.svg
            iconfont.ttf
            iconfont.woff
```

ajax 请求的实现步骤

Icons 页面为什么要使用二维数组?

### `Detail` 组件实现
目标效果:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191218211029.png)

项目结构:
```cmd
F:.
│  Detail.vue
│
└─components
        Banner.vue
        Header.vue
        List.vue
```

router 中 path 后的 :id 是什么用?

header 随着屏幕拖动逐渐显示的原理? (解绑全局事件)

公用组件的用处

如何使用递归组件, 为什么要使用?

### `City` 组件实现
目标效果:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191219005804.png)

项目结构:
```cmd
F:.
│  City.vue
│  
└─components
        Alphabet.vue
        Header.vue
        List.vue
        Search.vue
```

如何实现城市搜索

vuex 中为什么要使用 try catch?

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191219152002.png)

使用 vuex 修改 state 中一个值的流程

实现右侧字母表对应城市列表的原理.

### 项目的优化
localStorage

keep-alive 以及两种强制重新加载页面的方法

在 activated 中判断是否与上次的数据一致, 如果不一致则重新发送ajax 请求

exclude+组件名(组件里定义的name)

组件中name属性的作用
递归组件
取消缓存
vue-devtool调试时显示

多页面拖动会互相影响