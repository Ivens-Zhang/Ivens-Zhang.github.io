---
layout: post
title: "从零开始搭建属于自己的网站 (一)"
subtitle: '————域名、云服务器与 DNS'
author: "Ivens"
header-mask: 0.2
catalog: true
header-img: "img/in-post/2019-12-17/th.jpg"
tags:
  - 运维
---

好久没有写文章了, 一看上一次的文章还是一月四号的.

最近一直忙于弄服务器、网站、Linux 这上面. 花了挺多时间, 不过收获颇丰, 写下文章用以自勉.

## 一. 购买云服务器

云服务器上我选择用阿里云的服务器. 原因很简单:

1. 价格便宜
2. 技术出色
3. 搭配万网注册域名方便

如果只是用来学习 Linux 运维, 可以购买按小时收费的云服务器, 性价比则更高一些.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122163347.png)

*注意: 镜像选择镜像市场, 搜索宝塔面板, 选择排名第一的就可以.*

我们按照最低配置选购, 这样价格就是 `￥ 0.103 /时` 是不是还挺划算的. 周末实操一下 Linux 服务器, 5 个小时只要 5 毛钱.

当然, 如果你是希望长期放置主页、博客、项目的话, 还是推荐按年购买.

我是通过其他人分享的新人活动买的, 469元 2年, 我觉得还挺划算的. 环比了下其他几家的价格, 综合下来选择了阿里云.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/KZZJ0N3K8$@]X2O_X4H~JC0.png)

## 二. 域名

打开[**万网域名搜索**](https://wanwang.aliyun.com/domain/searchresult/?keyword=aaa&suffix=.com&domaintype=en#/?keyword=aaa&suffix=com), 输入你喜欢的域名.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122164300.png)

我们可以看到不同的后缀价格也不同, 如果只是用来练手, 可以挑一个便宜点的即可.

选好了添加到购物车, 点击`立即结算`, 后面跟着操作走即可.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122165407.png)

点击创建信息模板

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122165543.png)

按照流程即可, 最后回到购买页面, 付款完成.

## 三. 域名备案

域名备案是一个很麻烦的问题, 当然仅限于大陆.

阿里云可以帮我们省去很多麻烦, 这也是我推荐使用阿里云服务器的原因之一.

打开[**域名备案**](https://beian.aliyun.com/?spm=5176.10695662.amxosvpfn.19.72a05e10x5Udjr), 注意: 拉至最后, 看一下你注册地的规则, 比如在北京除了 `.com` 和 `.cn` 不可以注册其他的后缀域名.

这里推荐使用阿里云 APP 进行备案, 据说可以快几天的时间审批.

这里需要绑定阿里云服务器, 这是前面我推荐阿里云的一个原因, 因为我们买域名的万网和阿里云是一家的. 一家人干啥都方便些.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122203629.png)

后面按照流程填写信息即可.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122204017.png)

全部填写完提交申请, 阿里云官方会打电话先跟你核实一下这是初审, 初审通过会把你的备案信息提交给当地管局终审.

**终审大约 5 天左右, 备案就可以下来.**

## 云解析 DNS

当我们的域名审批下来, 就可以把域名进行解析了.

在阿里云的控制台中上方搜索 `云解析 DNS`, 将之前申请的域名添加进来.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122204507.png)

然后点击解析设置

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122204605.png)

这里新手建议直接点 `新手引导`, 填写自己的域名即可.

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122204947.png)

填写之前申请的阿里云主机 ip 地址

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122204947.png)

这时会自动生成两个解析, 一个 `www` 一个 `@` , 对应的是:
- **`www.AAAAAA.com`**
- **`AAAAAA.com`**

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122205039.png)

## 四. 上线主页

如果在购买服务器时没有选择宝塔面板, 那么我们可以打开[**宝塔 Linux 官网**](https://www.bt.cn/download/linux.html), 拉至底部根据自己的系统进行安装.

首先, 我们进入阿里云后台, 进入你的云服务器实例, 点击`本实例安全组 > 配置规则`, 开放 ``(8888|888|80|443|20|21)`` 这几个端口.

设置完成, 返回实例, 点击 `远程连接`用自己的账号登录, 在命令行中输入 `bt`, 完成设置

```cmd

Welcome to Alibaba Cloud Elastic Compute Service !

[root@iZuf69x6t7z31wzrbw4e5nZ ~]# bt
===============宝塔面板命令行==================
(1) 重启面板服务           (8) 改面板端口
(2) 停止面板服务           (9) 清除面板缓存
(3) 启动面板服务           (10) 清除登录限制
(4) 重载面板服务           (11) 取消入口限制
(5) 修改面板密码           (12) 取消域名绑定限制
(6) 修改面板用户名         (13) 取消IP访问限制
(7) 强制修改MySQL密码      (14) 查看面板默认信息
(22) 显示面板错误日志      (15) 清理系统垃圾
(23) 关闭BasicAuth认证     (16) 修复面板(检查错误并更新面板文件到最新版)
(24) 关闭谷歌认证          (17) 设置日志切割是否压缩
(25) 设置是否保存文件历史副本  (18) 设置是否自动备份面板
(0) 取消
===============================================
请输入命令编号：14
===============================================
正在执行(14)...
===============================================
==================================================================
BT-Panel default info!
==================================================================
Bt-Panel-URL: http://139.196.00.132:8888
username: aaaaa
password: c0c12b59
Warning:
If you cannot access the panel, 
release the following port (8888|888|80|443|20|21) in the security group
==================================================================
```

点击网址, 进入宝塔面板, 在软件商店中下载安装以下应用

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122211420.png)

点击 `宝塔一键部署源码`, 找到 `WordPress` 点击:

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122211635.png)

进入生成的网址, 填入账户名、密码、数据库名即可

![](https://raw.githubusercontent.com/Ivens-Zhang/PictureBed-2019.12.9/master/img/20200122211923.png)

现在, 我们的主页就上线成功了.

我们可以进入后台对我们的博客进行优化. `WordPress` 网上的教程很多, 我就不再赘述了.