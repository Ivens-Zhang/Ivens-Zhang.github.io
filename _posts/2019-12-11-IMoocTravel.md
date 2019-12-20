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

**项目的组件关系结构图:**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217160336.png)
## 搭建项目

**1. 项目前的准备工作, 如图安装好对应版本:**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217163927.png)

**2. 在 `cmd` 中输入 `vue ui` 使用网页端创建 `vue` 项目:**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/未命名_meitu_0.jpg)

**3. 打开 `GitHub Desktop`, 左上角菜单栏中 `File >> Create a new repository`, 选择项目所在目录即可.**

到这里, 我们的项目初步搭建就完成了.

### 项目结构
![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217174843.png)

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191217180643.png)

**上图是我们刚刚创建好的项目的结构, 说一些我的理解:**
- 因为`vue-cli`版本的变化, 不同的版本创建出的项目结构也会不同, 百度一下各个文件夹的作用很有意义.
- 在我这个版本中没有`config`文件夹, 如果要配置一些参数需要自己在`src`目录下创建`vue.config.js`文件.

***

### 初始化项目
**这是项目中需要用到依赖于第三方库:**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191220211241.png)

**首先, 为项目添加一些提前准备好的文件:**
1. `mock` 文件夹以及三个 json 文件
2. `styles` 文件夹以及两个 css 文件

**其次, 我们引入文件:**

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


### `Home` 组件实现
**目标效果:**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191218110944.png)

**组件结构:**

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

**`Iconfont` 的使用方法:**

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

**ajax 请求的实现步骤:**
1. 在 `Home.vue` 的 `script` 里添加引用 `axios`

```js
import axios from 'axios'
```

2. 在 `methods` 中添加两个方法

```js
getIndex () {
//这里请求的地址直接从 public 文件夹中寻找, 所以一开始的时候我们要把 json 文件放在里面.
 axios.get('/mock/index.json', {
     params: {
       city: this.$store.state.currentCity
     }  // 这里 get 中携带的第二个参数会在请求中携带参数, 如:
        // http://localhost:8080/mock/index.json?city=%E4%B8%8A%E6%B5%B7
   }).then(this.getIndexSucc)
   //如果有 get 请求发送成功则进入 then
 },
 getIndexSucc (res) {
   const resData = res.data.data
// 判断是否有返回的数据
   if (res.data) {
     // 提取返回 json 对象的数据, 赋给已经定义的变量
     this.swiperList = resData.swiperList
     this.iconList = resData.iconList
     this.recommendList = resData.recommendList
     this.weekendList = resData.weekendList
   }
 }
 ```

 3. 为 vue 文件添加 `mounted` 生命周期钩子, 保证每次被重新挂载都会发送 ajax 请求.

 ```js
mounted () {
 this.getIndex()
}     
```

***

**`Icons` 页面中轮播为什么要使用二维数组?**

要展示的效果如下:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191220225513.png)

如果放在一个数组里展示, 第 8 个以后的 `icon` 会超出边界消失, 并不会自动挪到第二个轮播页中. 所以我们要手动按照我们一页能放下的数量进行拆分.

如: `pages[2][3]`, 这就代表了第 3 页, 第 4 个图标.

在 `swiper-slide` 中做两次循环, 将数组完全遍历出来, 如下:

```html
<swiper-slide v-for="(page, index) in pages" :key="index">
  <div v-for="(innerItem, innerIndex) in page" :key="innerIndex">
    <img :src="innerItem.imgUrl">
  </div>
</swiper-slide>
```

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

**router 中 path 后加的 `:id` 是什么用?**

使用 `: + 参数名` 添加在 path 后占位, 当做路由参数. 这个参数可以在组件中区分不同显示不同的页面.


在组件的 `router-link` 标签中的使用方法:

```html
<router-link v-for="item of recommendList" :key="item.id" :to="'/detail/' + item.id">
```
或

```html
<router-link v-for="item of recommendList" :key="item.id" :to="{ name:'detail',params:{'id':item.id} }">
```

结果都是相同的, 地址栏会显示:
```
http://localhost:8080/detail/0001
```

关于获取路由参数可以参考这篇文章 [《Vue路由获取路由参数》](https://www.cnblogs.com/sese/p/9595625.html)

***

**`header` 会随着屏幕拖动逐渐显现的实现原理? **

首先, 逐渐显示的过程是元素的 `opacity` 由 0 → 1 的过程, 我们只需要让 `header` 的 `opacity` 跟随我们滚动页面的动作关联起来即可.

`document.documentElement.scrollTop` 参数可以帮我们记录现在页面距离顶部的距离, 也就是页面原始的位置为 0, 当我们向下拖拽页面时, 这个值会不断增加.

实现步骤: 
1. 在 data 中创建对象 `opacityStyle`, 用以绑定元素的 style.

```js
opacityStyle: {
  opacity: 0
}
```

2. 为页面滚动绑定监听事件
```js
mounted () {
 window.addEventListener('scroll', this.handleScroll)
}
```

3. 获取据顶部距离的高度, 并进行控制

```js
handleScroll () {
  let top = document.documentElement.scrollTop
  this.opacityStyle.opacity = top / 100
	// opacity 超过 1 还是完全不透明, 不会有什么影响.
}
```

***

**为何要解绑全局事件？**

如果我们不解绑全局事件, 如上面我们为 `window` 绑定的 `scroll` 事件.

当我们进入别的页面后拖动页面, 依旧会触发 `scroll` 事件, 所以我们要在组件内对全局事件进行解绑.

```js
deactivated () {
  window.removeEventListener('scroll', this.handleScroll)
}
```

***

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