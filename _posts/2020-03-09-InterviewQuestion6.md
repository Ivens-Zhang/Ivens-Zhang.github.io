---
title: "面向 2020 春招面试题(六)"
subtitle: "———— 高频面试题部分"
author: "Ivens"
header-mask: 0.1
header-img: "img/in-post/2019-12-4/th.jpg"
hidden: true
tags:
  - 春招面试题
---

## 几乎必问的问题

### webpack 的原理 ★

1. 合并 `shell` 传入和 `webpack.config.js` 文件配置的参数，生成 `compiler` 文件
2. 加载所有配置的插件, 构建声明周期
3. 找到配置的 `entry` 入口, 开始解析文件
4. 根据 `loader` 对文件进行递归转换
5. 得到包含所有依赖关系的 `chunk`
6. 根据配置的输出路径, 将内容写入文件系统

参考文章: [《 webpack系列--浅析webpack的原理 》](https://www.cnblogs.com/chengxs/p/11022842.html)

### XSS 和 CSRF 的防御 ★

`XSS`，即 `Cross Site Script`，中译是跨站脚本攻击；其原本缩写是 CSS，但为了和层叠样式表(Cascading Style Sheet)有所区分，因而在安全领域叫做 XSS。

XSS 攻击是指`攻击者在网站上注入恶意的客户端代码，通过恶意脚本对客户端网页进行篡改，从而在用户浏览网页时，对用户浏览器进行控制或者获取用户隐私数据`的一种攻击方式。

攻击者对客户端网页注入的恶意脚本一般包括 JavaScript，有时也会包含 HTML 和 Flash。有很多种方式进行 XSS 攻击，但它们的共同点为：将一些隐私数据像 `cookie、session` 发送给攻击者，将受害者重定向到一个由攻击者控制的网站，在受害者的机器上进行一些恶意操作。

XSS攻击可以分为3类：反射型（非持久型）、存储型（持久型）、基于DOM。

现在主流的浏览器内置了防范 XSS 的措施，例如 [CSP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)。但对于开发者来说，也应该寻找可靠的解决方案来防止 XSS 攻击。

#### ① HttpOnly 防止劫取 Cookie

HttpOnly 最早由微软提出，至今已经成为一个标准。浏览器将禁止页面的Javascript 访问带有 HttpOnly 属性的Cookie。

上文有说到，攻击者可以通过注入恶意脚本获取用户的 Cookie 信息。通常 Cookie 中都包含了用户的登录凭证信息，攻击者在获取到 Cookie 之后，则可以发起 Cookie 劫持攻击。所以，严格来说，HttpOnly 并非阻止 XSS 攻击，而是能阻止 XSS 攻击后的 Cookie 劫持攻击。

#### ② 输入检查

**不要相信用户的任何输入。** 对于用户的任何输入要进行检查、过滤和转义。建立可信任的字符和 HTML 标签白名单，对于不在白名单之列的字符或者标签进行过滤或编码。

在 XSS 防御中，输入检查一般是检查用户输入的数据中是否包含 `<`，`>` 等特殊字符，如果存在，则对特殊字符进行过滤或编码，这种方式也称为 XSS Filter。

而在一些前端框架中，都会有一份 `decodingMap`， 用于对用户输入所包含的特殊字符或标签进行编码或过滤，如 `<`，`>`，`script`，防止 XSS 攻击：

```js
// vuejs 中的 decodingMap
// 在 vuejs 中，如果输入带 script 标签的内容，会直接过滤掉
const decodingMap = {
  '&lt;': '<',
  '&gt;': '>',
  '&quot;': '"',
  '&amp;': '&',
  '&#10;': '\n'
}
```

#### ③ 输出检查

用户的输入会存在问题，服务端的输出也会存在问题。一般来说，除富文本的输出外，在变量输出到 HTML 页面时，可以使用编码或转义的方式来防御 XSS 攻击。例如利用 [sanitize-html](https://github.com/punkave/sanitize-html) 对输出内容进行有规则的过滤之后再输出到页面中。

---

`CSRF`，即 `Cross Site Request Forgery`，中译是跨站请求伪造，是一种劫持受信任用户向服务器发送非预期请求的攻击方式。

通常情况下，CSRF 攻击是攻击者借助受害者的 Cookie 骗取服务器的信任，可以在受害者毫不知情的情况下以受害者名义伪造请求发送给受攻击服务器，从而在并未授权的情况下执行在权限保护之下的操作。

> 很多同学会搞不明白XSS与CSRF的区别，虽然这两个关键词时常抱团出现，但他们两个是不同维度的东西（或者说他们的目的是不一样的）。XSS更偏向于方法论，CSRF更偏向于一种形式，只要是伪造用户发起的请求，都可成为CSRF攻击。
>
> 通常来说CSRF是由XSS实现的，也可以直接通过命令行模式（命令行敲命令来发起请求）直接伪造请求[只要通过合法验证即可]）。XSS更偏向于代码实现（即写一段拥有跨站请求功能的JavaScript脚本注入到一条帖子里，然后有用户访问了这个帖子，这就算是中了XSS攻击了），CSRF更偏向于一个攻击结果，只要发起了冒牌请求那么就算是CSRF了。

#### ① 验证码

验证码被认为是对抗 CSRF 攻击最简洁而有效的防御方法。

从上述示例中可以看出，CSRF 攻击往往是在用户不知情的情况下构造了网络请求。而验证码会强制用户必须与应用进行交互，才能完成最终请求。因为通常情况下，验证码能够很好地遏制 CSRF 攻击。

但验证码并不是万能的，因为出于用户考虑，不能给网站所有的操作都加上验证码。因此，验证码只能作为防御 CSRF 的一种辅助手段，而不能作为最主要的解决方案。

#### ② Referer Check

根据 HTTP 协议，在 HTTP 头中有一个字段叫 Referer，它记录了该 HTTP 请求的来源地址。通过 Referer Check，可以检查请求是否来自合法的"源"。

比如，如果用户要删除自己的帖子，那么先要登录 `www.c.com`，然后找到对应的页面，发起删除帖子的请求。此时，Referer 的值是 `http://www.c.com`；当请求是从 `www.a.com` 发起时，Referer 的值是 `http://www.a.com` 了。因此，要防御 CSRF 攻击，只需要对于每一个删帖请求验证其 Referer 值，如果是以 `www.c.com` 开头的域名，则说明该请求是来自网站自己的请求，是合法的。如果 Referer 是其他网站的话，则有可能是 CSRF 攻击，可以拒绝该请求。

针对上文的例子，可以在服务端增加如下代码：

```js
if (req.headers.referer !== 'http://www.c.com:8002/') {
    res.write('csrf 攻击');
    return;
}
```

[![referer check](https://user-images.githubusercontent.com/7871813/42734407-0f4c0728-8876-11e8-8565-21f89b01f6f0.png)](https://user-images.githubusercontent.com/7871813/42734407-0f4c0728-8876-11e8-8565-21f89b01f6f0.png)

Referer Check 不仅能防范 CSRF 攻击，另一个应用场景是 "防止图片盗链"。

#### ③ 添加 token 验证

CSRF 攻击之所以能够成功，是因为攻击者可以完全伪造用户的请求，该请求中所有的用户验证信息都是存在于 Cookie 中，因此攻击者可以在不知道这些验证信息的情况下直接利用用户自己的 Cookie 来通过安全验证。要抵御 CSRF，关键在于在请求中放入攻击者所不能伪造的信息，并且该信息不存在于 Cookie 之中。可以在 HTTP 请求中以参数的形式加入一个随机产生的 token，并在服务器端建立一个拦截器来验证这个 token，如果请求中没有 token 或者 token 内容不正确，则认为可能是 CSRF 攻击而拒绝该请求。

参考文章: 

- [《 浅说 XSS 和 CSRF 》](https://github.com/dwqs/blog/issues/68)

- [《 用大白话谈谈XSS与CSRF 》](https://segmentfault.com/a/1190000007059639#comment-area)



### 实现水平居中★

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200313224322.png)

#### ① 绝对定位 + 位移

有两种位移的方法:

1. margin
2. transform: translate

```css
/* margin */
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
  position: absolute;
  margin: 0 0 0 100px;
}
```

```css
/* transform: translate */
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
  position: absolute;
  transform: translate(100px, 0);
}
```

#### ② margin: 0 auto

```css
    #father {
      width: 300px;
      height: 300px;
      background: #ddd;
    }
    #child {
      width: 100px;
      height: 100px;
      background-color: green;
      margin: 0 auto;
    }
```

#### ③ 父元素text-align: center

```css
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
  text-align: center;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
  display: inline-block;
}
```

#### ④ 父元素flex布局 + 内容居中

```css
    #father {
      width: 300px;
      height: 300px;
      background: #ddd;
      display: flex;
      justify-content: center;
    }
    #child {
      width: 100px;
      height: 100px;
      background-color: green;
    }
```





### 实现垂直居中★

#### ① 绝对定位 + 移动具体数值

```css
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
  position: relative;
}

#child {
  width: 100px;
  height: 100px;
  background-color: green;
  position: absolute;
  top: 100px;
}
```

#### ② 绝对定位 + 负外边距

为什么要再加上负外边距, 因为绝对定位后我们让子元素 `top: 50%;` 但是元素是按照左上角来定位的, 所以现在就变成元素的左上角在中点上了, 所以我们还要再上移半个元素的高度, 使得元素整体处在垂直的中点.

```css
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
  position: relative;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
  position: absolute;
  top: 50%;
  margin: -50px 0 0 0 ;		/* 这里负外边距也可以使用百分比 */
}
```

```css
margin: style                  /*单值语法  所有边缘 */  举例： margin: 1em; 
margin: vertical horizontal    /*二值语法  纵向 横向 */  举例： margin: 5% auto; 
margin: top horizontal bottom  /*三值语法 上 横向 下*/  举例： margin: 1em auto 2em; 
margin: top right bottom left  /*四值语法 上 右 下 左*/  举例： margin: 2px 1em 0 auto; 
```

#### ③ 绝对定位 + translate() 调整

| 函数               | 描述                                   |
| :----------------- | :------------------------------------- |
| translate(*x*,*y*) | 定义 2D 转换，沿着 X 和 Y 轴移动元素。 |

```css
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
  position: relative;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
  position: absolute;
  top: 50%;
  transform: translate(0, -50px);
}
```

#### ④ 绝对定位 + margin: auto

这种实现方式的两个核心是：把要垂直居中的元素相对于父元素绝对定位，`top和bottom设为相等的值`，我这里设成了0，当然也可以设为 99999px 或者 -99999px 无论什么，只要两者相等就行，这一步做完之后再将要居中元素的 `margin` 属性值设为 `auto`，这样便可以实现垂直居中了。此外这个方法也可以用于左右水平居中, 只需要设置 `left和right设为相等的值` 即可.

```css
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
  position: relative;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto;
}
```

#### ⑤ 父元素padding

```css
#father {
  width: 300px;
  height: 100px;
  background: #ddd;
  padding: 100px 0;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
}
```

P.S. 注意: 使用这种方法父元素的高度的设置要小心, 父元素的高度应该 = 要居中子元素的高度, 因为 `padding` 会自动撑起父元素的高度. 如果我们让父元素 = 子元素高度 + 上内边距 + 下内边距, 这样反而会出错.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200313115058.png)

#### ⑥ 父元素flex布局 + align-items: center

```css
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
  display: flex;
  align-items: center;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
}
```

#### ⑦ 父元素flex布局 + 改变主轴

这种方式也是首先给父元素设置 `display:flex;` 设置好之后改变主轴的方向 `flex-direction: column;` 该属性可能的取值有四个，分别如下：  

- row（该值为默认值）：主轴为水平方向，起点在左端
- row-reverse：主轴为水平方向，起点在右端
- column：主轴为垂直方向，起点在上沿
- column-reverse：主轴为垂直方向，起点在下沿

`justify-content` 属性定义了项目在主轴上的对齐方式，可能的取值有五个，分别如下（具体的对齐方式与主轴的方向有关，以下假定主轴方向为默认的从左到右）：

- flex-start（该值是默认值）：左对齐
- flex-end：右对齐
- center：居中对齐
- space-between：两端对齐，各个项目之间的间隔均相等
- space-around：各个项目两侧的间隔相等

```css
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
}
```

#### ⑧ 父元素 -webkit-box 布局

```css
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
  display: -webkit-box;
  -webkit-box-align: center;
  -moz-box-pack: center;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
}
```

#### ⑨ 父元素 table-cell 布局

- **table：**会作为块级表格来显示（类似 <table>）

- **table-row：**浏览器会默认创建一个表格行（类似<tr>）

- **table-cell：**作为一个表格单元格显示（类似 <td> 和 <th>）

```css
#father {
  width: 300px;
  height: 300px;
  background: #ddd;
  display: table-cell;
  vertical-align: middle;
}
#child {
  width: 100px;
  height: 100px;
  background-color: green;
}
```









url 页面请求的过程 (输入网址后的每一步) ★

Vue 双向数据绑定及原理 ★

Vue 的数据响应式原理 ★

跨域 ★

### http 状态码 ★

当请求被发送到服务器时，我们需要执行一些基于响应的任务, `readyState` 属性存有 XMLHttpRequest 的状态信息。每当 readyState 改变时，就会触发 onreadystatechange 事件。

| 属性                 | 描述                                                         |
| :------------------- | :----------------------------------------------------------- |
| `onreadystatechange` | 存储函数（或函数名），每当 `readyState` 属性改变时，就会调用该函数。 |
| `readyState`         | 存有 XMLHttpRequest 的状态, 从 0 到 4 发生变化。<br/>0: 请求未初始化<br/>1: 服务器连接已建立<br/>2: 请求已接收3: 请求处理中<br/>4: 请求已完成，且响应已就绪 |

当浏览者访问一个网页时，浏览者的浏览器会向网页所在服务器发出请求。当浏览器接收并显示网页前，此网页所在的`服务器会返回一个包含HTTP状态码的信息头（server header）`用以响应浏览器的请求。

下面是`常见`的HTTP状态码：

- 200 - 请求成功
- 301 - 资源（网页等）被永久转移到其它URL
- 404 - 请求的资源（网页等）不存在
- 500 - 内部服务器错误

HTTP状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型，后两个数字没有分类的作用。HTTP状态码共分为5种类型：

<center>HTTP状态码分类:</center>

| 分类 | 分类描述                                       |
| :--- | :--------------------------------------------- |
| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| 2**  | 成功，操作被成功接收并处理                     |
| 3**  | 重定向，需要进一步的操作以完成请求             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |



<center>HTTP状态码列表:</center>

| 状态码 | 状态码英文名称                  | 中文描述                                                     |
| :----- | :------------------------------ | :----------------------------------------------------------- |
| 100    | Continue                        | 继续。[客户端](http://www.dreamdu.com/webbuild/client_vs_server/)应继续其请求 |
| 101    | Switching Protocols             | 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议 |
|        |                                 |                                                              |
| 200    | OK                              | 请求成功。一般用于GET与POST请求                              |
| 201    | Created                         | 已创建。成功请求并创建了新的资源                             |
| 202    | Accepted                        | 已接受。已经接受请求，但未处理完成                           |
| 203    | Non-Authoritative Information   | 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本 |
| 204    | No Content                      | 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档 |
| 205    | Reset Content                   | 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域 |
| 206    | Partial Content                 | 部分内容。服务器成功处理了部分GET请求                        |
|        |                                 |                                                              |
| 300    | Multiple Choices                | 多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择 |
| 301    | Moved Permanently               | 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替 |
| 302    | Found                           | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
| 303    | See Other                       | 查看其它地址。与301类似。使用GET和POST请求查看               |
| 304    | Not Modified                    | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
| 305    | Use Proxy                       | 使用代理。所请求的资源必须通过代理访问                       |
| 306    | Unused                          | 已经被废弃的HTTP状态码                                       |
| 307    | Temporary Redirect              | 临时重定向。与302类似。使用GET请求重定向                     |
|        |                                 |                                                              |
| 400    | Bad Request                     | 客户端请求的语法错误，服务器无法理解                         |
| 401    | Unauthorized                    | 请求要求用户的身份认证                                       |
| 402    | Payment Required                | 保留，将来使用                                               |
| 403    | Forbidden                       | 服务器理解请求客户端的请求，但是拒绝执行此请求               |
| 404    | Not Found                       | 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
| 405    | Method Not Allowed              | 客户端请求中的方法被禁止                                     |
| 406    | Not Acceptable                  | 服务器无法根据客户端请求的内容特性完成请求                   |
| 407    | Proxy Authentication Required   | 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权 |
| 408    | Request Time-out                | 服务器等待客户端发送的请求时间过长，超时                     |
| 409    | Conflict                        | 服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突 |
| 410    | Gone                            | 客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置 |
| 411    | Length Required                 | 服务器无法处理客户端发送的不带Content-Length的请求信息       |
| 412    | Precondition Failed             | 客户端请求信息的先决条件错误                                 |
| 413    | Request Entity Too Large        | 由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息 |
| 414    | Request-URI Too Large           | 请求的URI过长（URI通常为网址），服务器无法处理               |
| 415    | Unsupported Media Type          | 服务器无法处理请求附带的媒体格式                             |
| 416    | Requested range not satisfiable | 客户端请求的范围无效                                         |
| 417    | Expectation Failed              | 服务器无法满足Expect的请求头信息                             |
|        |                                 |                                                              |
| 500    | Internal Server Error           | 服务器内部错误，无法完成请求                                 |
| 501    | Not Implemented                 | 服务器不支持请求的功能，无法完成请求                         |
| 502    | Bad Gateway                     | 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应 |
| 503    | Service Unavailable             | 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中 |
| 504    | Gateway Time-out                | 充当网关或代理的服务器，未及时从远端服务器获取请求           |
| 505    | HTTP Version not supported      | 服务器不支持请求的HTTP协议的版本，无法完成处理               |



防抖与节流(手写防抖)★

#### CSS 实现两列布局 ★

#### 方法一 : 绝对定位 + margin-left

这里的 `margin-left` 只是一种方法, 只要是可以让元素偏移都可以, 如 `transform: transla()`.

```css
*{
  margin: 0;
  padding: 0;
}
.right {
  width: 200px;
  height: 500px;
  position: absolute;
  background-color: green;
  top: 0;
  left: 0;
}
.left{
  height: 500px;
  background-color: yellow;
  margin-left: 200px;
}
```

为什么这里要重置 css ?

如果不加上 `*{ margin: 0;  padding: 0; }`, 页面会这样显示:

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200315115538.png" style="zoom:67%;" />

红框标记的是不受控制的区域, 这些错乱出现的原因是浏览器会有一些默认样式, 这会影响到我们的布局.

#### 方法二: float + margi-left

与方法一大同小异, 核心思想都是一边定宽一边自适应.

```css
*{
  margin: 0;
  padding: 0;
}
.left {
  width: 200px;
  height: 500px;
  background-color: green;
  float: left;
}
.right{
  height: 500px;
  background-color: yellow;
  margin-left: 200px;
}
```

#### 方法三: table 布局

```html
<style>
  * {
    margin: 0;
    padding: 0;
  }

  .left {
    width: 200px;
    height: 500px;
    background-color: green;
  }

  .right {
    height: 500px;
    width: 500px;
    background-color: yellow;
  }
</style>

<table>
  <tr>
    <td><div id="child1" class="left"></div></td>
    <td><div id="child2" class="right"></div></td>
  </tr>
</table>
```

#### 方法四: flex 布局

```css
* {
  margin: 0;
  padding: 0;
}

.father {
  display: flex;
}

.left {
  width: 200px;
  height: 500px;
  flex: 1;
  background-color: green;
}

.right {
  height: 500px;
  flex: 1;
  background-color: yellow;
}
```





#### CSS 实现三列布局 ★



页面性能优化 ★

1. 合并请求, 减少浏览器对服务器发起的请求数，从而减少在过程中花费的时间。
2. 开启KeepAlive (保持连接) & 开启Gzip (服务器端压缩资源)
3. 非核心代码异步加载异步加载(async, defer)
4. 利用浏览器缓存 (强缓存, 协商缓存)
5. CDN 加速

## HTML 相关问题

### html5 新增特性

- 语义标签

  - 使开发者更方便清晰构建页面的布局

- 增强型表单

- 视频和音频

  - 音频：`<audio src=" "></audio>`
  - 视频：`<video src=" "></video>`

- `Canvas` 绘图与 `SVG` 绘图

  - SVG与Canvas区别

    *SVG适用于描述XML中的2D图形的语言

    *Canvas随时随地绘制2D图形（使用javaScript）

    *SVG是基于XML的，意味这可以操作DOM，渲染速度较慢

    *在SVG中每个形状都被当做是一个对象，如果SVG发生改变，页面就会发生重绘

    *Canvas是一像素一像素地渲染，如果改变某一个位置，整个画布会重绘。

    | Canvas                           | SVG                        |
    | -------------------------------- | -------------------------- |
    | 依赖分辨率                       | 不依赖分辨率               |
    | 不支持事件处理器                 | 支持事件处理器             |
    | 能够以.png或.jpg格式保存结果图像 | 复杂度会减慢搞渲染速度     |
    | 文字呈现功能比较简单             | 适合大型渲染区域的应用程序 |
    | 最合适图像密集的游戏             | 不适合游戏应用             |

- 地理定位

  - 使用`getCurrentPosition()`方法来获取用户的位置。以实现“LBS服务”

- 拖放 API

  - `draggable` 属性

- WebWorker

  - 通过加载一个脚本文件，进而创建一个独立工作的线程，在主线程之外运行.

- WebStorage

  - localStorage
  - sessionStore

- WebSocket

详细内容参考: [《HTML5 新特性总结》](https://www.cnblogs.com/binguo666/p/10928907.html)

---

HTML5 元素分类:

- 按默认样式分

| 按默认样式分          | 常见元素                                                   |
| --------------------- | ---------------------------------------------------------- |
| 块级 block            | 区块元素, 如: `div、p、ul、ol、li、form、h1~h6、table`     |
| 行内 inline           | 文本相关标签, 如: `span、a、strong、em、i、del、s、ins、u` |
| 行内块级 inline-block | 表单元素, 如:  `input、 img、td`                           |

- 按内容分

<img src="https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20191222160615.png" style="zoom:67%;" />

**`Metadata`** <br>
顾名思义，Metadata元素意指那些定义文档元数据信息的元素 — 其作用包括：影响文档中其它节点的展现与行为、定义文档与其它外部资源之间的关系等。以下元素属于 `Metadata：base, link, meta, noscript, script, style, template, title`

**`Flow`** <br>
所有可以放在body标签内，构成文档内容的元素均属于Flow元素。

**`Sectioning`** <br>
指定义页面结构的元素，具体包含以下四个：`article, aside, nav, section`

**`Heading`** <br>
所有标题元素属于 Heading，也即以下6个元素：`h1, h2, h3, h4, h5, h6`

**`Phrasing`** <br>
所有可以放在p标签内，构成段落内容的元素均属于 Phrasing 元素。比如：`em、strong` ，另外如：`audio、video、img、select、input` 等元素(经测试，这些元素都可以放置在p标签中)。Phrasing 元素内部一般只能包含别的 Phrasing 元素。


**`Embedded`** <br>
所有用于在网页中嵌入外部资源的元素均属于Embedded元素，具体包含以下9个：`audio, video, img, canvas, svg, iframe, embed, object, math`

**`Interactive`** <br>
所有与用户交互有关的元素包括 `a, input, textarea, select` 等。

细节参考:  [《W3C HTML5标准阅读笔记 – 元素分类》](https://www.zhangjinglin.cn/blog/d37dbcd34db6df9e7adfbe3ae2d8ad95.html)

***

**HTML 元素的嵌套关系**

- 块级元素可以包含行内元素
- 块级元素不一定能包含块级元素
- 行内元素一般不能包含块级元素
  - 反例: `a` 可以包含 `div`

---

**HTML 的默认样式**

- 每个浏览器都会自带默认样式, 但是在开发中会造成页面不统一的问题.
- 解决方案: CSS Reset
  - `CSS Tool: Reset CSS` & `YUI: CSS Reset` --- 清除浏览器默认样式 
  - `Normalize.css` --- 统一各浏览器默认样式
  - *{margin:0;padding:0} --- 最简单方法, 可能存在性能浪费

---

**真题:**

`DOCTYPE` 的意义?

- 让浏览器以标准模式渲染, 如: 盒子模型宽度是否包含 padding 值.
- 让浏览器知道元素的合法性, 如: 在 XHTML 中标签必须完全闭合.

**HTML / XHTML / HTML5 的关系**

- HTML 属于 SGML
- XHTML 是对 HTML 进行 XML 严格化的结果
- HTML5 不属于 SGML 或 XML, 是一种独立的规范

**em 和 i 的区别**

- `em` 表示强调, `i` 纯样式标签表示斜体
- 在 HTML5 中 `i` 一般用作图标

**语义化的意义**

- 代码可读性更强, 容易维护
- 浏览器容易理解结构
- 有助于 SEO

**哪些元素可以自闭合**

- 表单元素: input
- img
- br / hr
- head 中的 meta / link

**HTML 与 DOM 的关系**

- DOM 是由 HTML 解析而来的
- JavaScript 维护的是 DOM 而不是 HTML






http 缓存机制 / 协议 / 优化

webSocket 基础理解

http 2.0 与 1.0 的区别

http 和 https 的区别

### token

参考文章: 

- [《 一文彻底搞懂Cookie、Session、Token到底是什么 》](https://juejin.im/post/5de4c3c76fb9a071b86cc482#heading-6)
- [《 JSON Web Token 入门教程- 阮一峰的网络日志 》](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)

## CSS 相关问题

圣杯布局 / 双飞翼布局

两者实现的结果都是一样的, 只是在思路上有所区别. 用自己的话总结就是:

- "圣杯是main腾好了地方给两边, 双飞翼是main乖乖地给两边让位置."

[《 CSS布局 -- 圣杯布局 & 双飞翼布局 》](https://www.cnblogs.com/imwtr/p/4441741.html)

CSS 媒体查询

position 取值与区别

清除浮动

transition 和 animation

CSS 里的 flex

display 的取值和区别

#### 几种图片格式的区别

- `GIF`
  - `8`位像素，`256`色
  - 无损压缩
  - 支持简单`动画`
  - 支持`boolean`透明
  - 适合简单动画
- `JPEG`
  - 颜色限于`256`
  - `有损压缩`
  - 可控制压缩质量
  - `体积比png小`
  - 不支持透明
  - 适合照片
- `PNG`
  - 有`PNG8`和`truecolor PNG`
  - `无损压缩`
  - `PNG8`类似`GIF`颜色上限为`256`，文件小，支持`alpha`透明度，无动画
  - 适合图标、背景、按钮

### 一像素边框

- 解决思路 : `伪类 + transform`

```
li{position: relative;}
li:after{
  position: absolute;
  bottom:0;
  left:0;
  content: '';
  width:100%;
  height:1px;
  border-top:1px solid black;
  transform: scaleY(0.5);//注意兼容性
}
```

这种方式的原理很简单，就是把原先元素的 border 去掉，然后利用 `:before` 或者 `:after` 重做 `border` ，并 `transform` 的 `scale` 缩小一半，原先的元素相对定位，新做的 border 绝对定位。个人认为这是比较完美的做法。

## JavaScript 相关问题

继承的理解

new 的思路

### 正则

正则编写步骤:

1. 将规则拆分
2. 逐个翻译为正则表达式
3. 拼接各表达式

[《 JS正则表达式入门，看这篇就够了 》](https://segmentfault.com/a/1190000009324194)

#### typeof 和 instanceOf 的区别

typeof是一个用来检测变量数据类型的操作符，主要用来检测基本数据类型。

为什么这么说呢？因为它可以判断一个变量是字符串、数值、布尔值还是 undefined，但是如果检测的变量是引用数据类型，可以返回object或者function，`但不能细致地分出是array还是RegExp`。

而 instanceof主要的目的就是检测引用类型,它可以判断出对象是Array还是RegExp。

总结起来就是：

**`typeof` 和 `instanceof` 都是用来检测变量类型的操作符,二者的区别在于 `typeof` 一般是检测基本数据类型,而 `instanceof` 是用来检测引用数据类型。**

*P.S. `typeof`几乎不可能得到它们想要的结果。`typeof`只能用来粗略检测, 或者只有一个实际应用场景，就是**用来检测一个对象是否已经定义或者是否已经赋值**。*

| Value              | Class    | Type                          |
| ------------------ | -------- | ----------------------------- |
| "foo"              | String   | string                        |
| new String("foo")  | String   | object                        |
| 1.2                | Number   | number                        |
| new Number(1.2)    | Number   | object                        |
| true               | Boolean  | boolean                       |
| new Boolean(true)  | Boolean  | object                        |
| new Date()         | Date     | object                        |
| new Error()        | Error    | object                        |
| [1,2,3]            | Array    | object                        |
| new Array(1, 2, 3) | Array    | object                        |
| new Function("")   | Function | function                      |
| /abc/g             | RegExp   | object (function in Nitro/V8) |
| new RegExp("meow") | RegExp   | object (function in Nitro/V8) |
| {}                 | Object   | object                        |
| new Object()       | Object   | object                        |

上面表格中，Type 一列表示 typeof 操作符的运算结果。可以看到，这个值在大多数情况下都返回 "object"。



手写一个 promise

图片懒加载

diff (可能手写)

事件循环

ES6 的 async 和 await 和 promise

深拷贝和浅拷贝

全局变量污染的解决

快速排序和二分查找



## Vue 相关问题

Vue 子父组件之间的通信

mvc 和 mvp 和 mvvm

babel 原理 (简单了解)



## 计算机网络相关问题

三次握手, 四次挥手

tcp 和 udp






