---
layout: post
title: "Vue 中使用 Echarts 的步骤"
subtitle: ""
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-12-4/th.jpg"
hidden: true
tags:
  - Vue.js
---

### 1. 安装

```cmd
$ npm install -S echarts
```

### 2. 全局引用

在 `Vue` 项目 `src` 文件夹中的 `main.js` 进行引用.

```js
import echarts from 'echarts'
Vue.prototype.$echarts = echarts	// 给 Vue 原型绑定 echarts, 以后使用更方便
```

### 3. 实现

1. 我们在 `views` 文件夹中新创建一个 `demo.vue` 文件.
2. 创建一个 `div` 盛放我们的表格.

```vue
<template>
  <!--为echarts准备一个具备大小的容器dom-->
  <div id="main" style="width: 600px;height: 400px;">aaa</div>
</template>
```

3. 创建方法

我们先看下 `Echarts` 官网的实例:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200401120516.png)

可以看出, 设置中主要分了四部分: 

- **title				`标题`**
- **tooltip         `鼠标悬浮显示`**
- **legend         `侧边图例`**
- **series           `具体参数`**

```js
drawPie(id) {
  // 这里使用第 2 步中绑定到原型上的 echarts 对象.
  this.$echarts.init(document.getElementById(id)).setOption({
    title: {		// 标题
      text: "某站点用户访问来源",
      subtext: "纯属虚构",
      left: "center"
    },
    tooltip: {	// 鼠标悬浮显示
      trigger: "item",
      formatter: "{a} <br/>{b} : {c} ({d}%)"
    },
    legend: {		// 侧边图例
      orient: "vertical",
      left: "left",
      data: ["直接访问", "邮件营销", "联盟广告", "视频广告", "搜索引擎"]
    },
    series: [		// 具体参数
      {
        name: "访问来源",
        type: "pie",
        radius: "55%",
        center: ["50%", "60%"],
        data: [
          { value: 335, name: "直接访问" },
          { value: 310, name: "邮件营销" },
          { value: 234, name: "联盟广告" },
          { value: 135, name: "视频广告" },
          { value: 1548, name: "搜索引擎" }
        ],
        emphasis: {
          itemStyle: {
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: "rgba(0, 0, 0, 0.5)"
          }
        }
      }
    ]
  });
}
```

4. 调用

```js
// 挂载到生命周期函数
mounted() {
  this.$nextTick(function() {
    this.drawPie("main");
  });
}
```



























