## Aichef-web 自适应研究报告

### 一. 相关概念

#### 响应式设计（Responsive design）：

> [百科]：响应式设计是Ethan Marcotte在2010年5月份提出的一个概念，简而言之，就是一个网站能够兼容多个终端—而不是为每个终端做一个特定的版本。这个概念是为解决移动互联网浏览而诞生的。

通过检测视口分辨率，来判断当前访问的设备是：pc端、平板、手机，从而请求服务层，`返回不同的页面`.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200413164317.png)

#### 自适应设计（Adaptive Design）

> [百科]：自适应设计指能使网页自适应显示在不同大小终端设备上新网页设计方式及技术。

响应式设计可以根据屏幕分辨率自适应以及自动缩放图片、自动调整布局，这不只是技术的实现，更多的是对于设计的全新思维模式。

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/a.jpg)

这是我博客的主页, 左图是屏幕宽度大于 800px 时的布局, 右图是小于 800px 时的布局.



### 二. 通常做法

- **`隐藏 + 折行 + 自适应空间`**

这是适配小屏布局思路, 响应式布局设计流程:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200101235353.png)

#### 常用的实现方式:

1. **首先，在网页代码的头部，加入一行viewport元标签。**

 `viewport`是网页默认的宽度和高度，上面这行代码的意思是，网页宽度默认等于屏幕宽度（width=device-width），原始缩放比例（initial-scale=1）为1.0，即网页初始大小占屏幕面积的100%。

所有主流浏览器都支持这个设置，包括IE9。对于那些老式浏览器（主要是IE6、7、8），需要使用**`css3-mediaqueries.js`** 



2. **尽量少使用绝对宽度**

**由于网页会根据屏幕宽度调整布局，所以不能使用绝对宽度的布局，也不能使用具有绝对宽度的元素。这一条非常重要。**

通过指定百分比宽度来替代：

```css
width: xx%;
or
width:auto;
```

 

3. **相对大小的字体**

字体也不能使用`绝对大小（px）`，而只能使用`相对大小（em）`或者`高清方案（rem）`,rem不局限于字体大小，前面的宽度width也可以使用，代替百分比。

**`em` 与 `rem` 的区别?**

`em` 的值并不是固定的，它会继承父级元素的字体大小。如果当前对行内文本的字体尺寸未被设置，则相对于浏览器的默认字体尺寸。

 `rem` 相对大小相对的只是HTML根元素, 不会受到父级的影响。目前，除了 `IE8` 及更早版本外，所有浏览器均已支持rem。

 **注意: 在搭配 `media 媒体查询`使用 `rem` 时, 记得给 `html` 选择器的 `font-size` 属性设置一个绝对大小的值.**

```css
body {
　　font: normal 100% Helvetica, Arial, sans-serif;
}
```

上面的代码指定，字体大小是页面默认大小的100%，即16像素。

```css
h1 {
　　font-size: 1.5em; 
}
```

然后，h1的大小是默认大小的1.5倍，即24像素（24/16=1.5）。

```css
small {
　　font-size: 0.875em;
}
```

small元素的大小是默认大小的0.875倍，即14像素（14/16=0.875）。

 

4. **流动布局（fluid grid）**

**“流动布局”的含义是，各个区块的位置都是浮动的，不是固定不变的。**

```css
.main {
  float: right;
  width: 70%; 
}

.leftBar {
  float: left;
  width: 25%;
}
```

float的好处是，如果宽度太小，放不下两个元素，后面的元素会自动滚动到前面元素的下方，不会在水平方向`overflow（溢出）`，避免了水平滚动条的出现。

 

5. **选择加载CSS**

`"自适应网页设计"的核心，就是CSS3引入的Media Jquery模块。`它的意思就是:

- 自动探测屏幕宽度
- 加载相应的CSS文件

```html
<!-- 如果屏幕宽度小于400像素（max-device-width: 400px），就加载`tinyScreen.css`文件。 -->
<link rel="stylesheet" type="text/css"
　　　media="screen and (max-device-width: 400px)"
　　　href="tinyScreen.css" />
<!-- 用 @import 写法也可以 -->
@import  url("tinyScreen.css") screen and (max-device-width: 400px);
```

```html
<!-- 如果屏幕宽度在400像素到600像素之间，则加载`smallScreen.css`文件。 -->
<link rel="stylesheet" type="text/css"
　　media="screen and (min-width: 400px) and (max-device-width: 600px)"
　　href="smallScreen.css" />
```

同一个CSS文件中，也可以根据不同的屏幕分辨率，选择应用不同的CSS规则:

```
@media  screen and (max-device-width: 400px) {
   .column {
     float: none;
     width:auto;
   }

   #sidebar {
     display:none;
   }
 }
```

上面的代码意思是，如果屏幕宽度小于400像素，则`column块取消浮动（float:none）`、`宽度自动调节（width:auto）`、`sidebar块不显示（display:none）`。



6. **图片的自适应（fluid image）**

除了布局和文本，"自适应网页设计"还必须实现图片的自动缩放。

```css
// 只要一行CSS代码
img { max-width: 100%;}

// 对于大多数嵌入网页的视频也有效，所以可以写成：
img, object { max-width: 100%;}

// 老版本的IE不支持max-width，所以只好写成：
img { width: 100%; }

// windows平台缩放图片时，可能出现图像失真现象。这时，可以尝试使用IE的专有命令：
img { -ms-interpolation-mode: bicubic; }
```



### 三. 主流网站的自适应模式

#### 百度 & Google

​		a)    优点: 简单稳定, 使用外边距百分比设置, 在一开始缩放网页时有不错的体验

​		b)    缺点: 一旦宽度小于指定宽度, 浏览器下部会出现滚动条 

原理：

- 把所有控件放在一个div里，然后把最外层这个div设置样式为 左右上方 有`百分比`的空白

```html
<div style="min-width:1050px;min-height:100px;margin-right:2%;margin-left:2%">
...  页面内控件 ...
</div>
```

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/213546465465.jpg)

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/1234564447161.png)



#### 淘宝 & 亚马逊

| 1250 < w                                                     | 1155 < w < 1250                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/11235241.png" style="zoom: 33%;" /> | <img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/274145685.png" style="zoom: 33%;" /> |
| **1010 < w < 1155**                                          | **w < 1010**                                                 |
| <img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/3741852987.png" style="zoom: 33%;" /> | <img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/4123852789.png" style="zoom: 50%;" /> |



| 宽度                                 | 页面                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| **w < 1205  页面两侧缩减**           | ![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/2.png) |
| **1000 < w < 1205  content区域调整** | ![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/1.png) |
| **w  < 1000 与亚马逊一样右侧看不到** | ![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/3.png) |

#### 京东

京东的响应式做的比较完善:

| 宽度                | 页面                                                         |
| ------------------- | ------------------------------------------------------------ |
| **1330 < w**        | <img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/1123.png"  /> |
| **1145 < w < 1330** | ![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/2852.png) |
| **w < 1145**        | ![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200414140603.png) |

### 四. 相关建议和经验

自适应布局可以让你的设计更加可控，因为你只需要考虑几种状态就可以了。

但在响应式布局中你可能需要面对非常多状态——是的，大部分状态之间的区别很小，但它们又的确是不同的，这样一来就很难确切搞清你的设计会是什么样。

同时这也带来了测试上的难题，你很难有绝对的把握预测到它会怎样。

换个角度说，这也是响应式布局的魅力所在。相比较来说自适应布局有它自己的优势，因为它们实施起来代价更低，测试更容易，这往往让他们成为更切实际的解决方案。

其实，无论是哪种设计理念都是各有优缺点，具体的选择还是`要从团队/项目实际需求出发去选择`。

在看 aichef-web 这个项目自适应的设计时, 我发现周颖颖老师在设计时考虑到了很多种宽度. 我惊讶地发现, 管理员首页就有 8 种根据宽度不同改变样式的情况(图3), 并且设置了最小宽度为 500px, 当我再缩小浏览器窗口时便会像百度首页一样底部出现滚动条.

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200414140855.png)

而且在其他页面中我也看到了有根据宽度变化布局改变的情况. 这里不排除有 Quasar 所自带的自适应样式, 如果**按照项目的时间要求下(两周10个工作日) 要完成这么多种的响应式设计的适配其实是很难的**, 而且现在已经设置了最小宽度为 500px, 如果我们直接找到第一次发生布局变化的临界值, 设置为最小宽度, 虽然会在底部出现滚动条, 或许这样更适合现在的进度要求.

