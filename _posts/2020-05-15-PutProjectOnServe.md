## 本地web项目部署服务器流程

> **调试环境:**
>
> 1. **本地主机: Windows 10 Version 1903**
> 2. **远程服务器: CentOS 7. 7**

### 一、找一个 SSH 工具

**什么是 SSH ？**

> SSH 为 [Secure Shell](https://baike.baidu.com/item/Secure Shell) 的缩写，由 IETF 的网络小组（Network Working Group）所制定；SSH 为建立在应用层基础上的安全协议。SSH 是较可靠，专为[远程登录](https://baike.baidu.com/item/远程登录/1071998)会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。SSH最初是UNIX系统上的一个程序，后来又迅速扩展到其他操作平台。SSH在正确使用时可弥补网络中的漏洞。SSH客户端适用于多种平台。几乎所有UNIX平台—包括[HP-UX](https://baike.baidu.com/item/HP-UX)、[Linux](https://baike.baidu.com/item/Linux)、[AIX](https://baike.baidu.com/item/AIX)、[Solaris](https://baike.baidu.com/item/Solaris/3517)、[Digital](https://baike.baidu.com/item/Digital) [UNIX](https://baike.baidu.com/item/UNIX)、[Irix](https://baike.baidu.com/item/Irix)，以及其他平台，都可运行SSH。

所以，说白了 `SSH` 就是能让你通过命令行的形式, 远程连接其他主机的协议.

市面上有很多 `SSH` 工具可供挑选, 如:

- Putty
- XShell
- ... ...

此外 `Mac OS` 可以不需要借助工具直接 `SSH` 其他主机. `Git Bash` 也可以直接输入 `ssh 用户名@远程主机地址` 进行操作.

这里推荐使用 `Putty`, 没别的, 就因为免费.



### 二、开始连接

第一步, 打开 `Putty`, 让我们看看界面:

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200515160537.png)

这里有一个关键点, 当你要使用 `SSH` 连接其他主机的时候, 对方的主机一定是要开放了 22 端口的.

填写好了就可以点击 `Open` 了.

![image-20200515160918702](C:\Users\zhangyi\AppData\Roaming\Typora\typora-user-images\image-20200515160918702.png)

需要你填写要登录的角色, 一般生产环境中不会给你 `root` 的, 因为权限太大. 此外, 输入密码的时候界面上也不会显示你的密码, 甚至连 `*` 都没有, 为了安全可以理解.

连进去以后你得想想接下来该干什么了, 无非这几步:

1. 把 `webpack` 打包好的 `dist/` 放到服务器上
2. 用 `Nginx` 把他启动起来

说是简单的两步, 其实里面的坑大了去了......

### 三、Win-To-Linux 传输文件

把打包好了的 `dist/` 文件夹, 传到一个远程的主机上, 如果这是日常在两个 Windows 上你会怎么做?

打开 `qq`, 把文件拉进去, 对方接收一下即可. 或者是, 掏出挂在钥匙环上的 U盘, 两边拷贝一下.

但是不好意思, 我甚至都不知道公司的服务器放在那栋楼里了, 所以, 物理层面的拷贝就算了吧. 想想有没有什么东西能帮我们实现这个操作.

推荐一个应用:

- XFtp

和上面那个 `XShell` 是一家的, 两个都有一个缺点——要钱！当然, 你也可以选择下个破解版的, 这都没啥大碍.

这里推荐一种看起来更优雅一些的解决方法:

- **`scp` 指令**

`scp`是 `secure copy` 的缩写,  `scp` 是 `linux` 系统下基于 `ssh` 登陆进行安全的远程文件拷贝命令, 接下来看看如何实现的.

1. 在任务管理器中找一个文件, 打开属性复制一下他的地址
2. 右键选择 `Git Bash Here` 

当然, 这一步的前提是你提前安装好了 Git, 我没有试用自带的 `PowerShell` 或者 `cmd` 能不能做到, 但是都想着要把东西部署到服务器上了, `Git` 总该是有了的吧.

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200515162731.png)

好了, 如果你这个文件时真存在的, 那这个时候会让你输入密码了.

输好, 回车, 打开 `Putty` 看看, 发现文件已经在那了.

书面化写一下四条指令:

```cmd
# 上传单个文件
scp local_file remote_username@remote_ip:remote_folder
# 上传文件夹
scp -r local_folder remote_username@remote_ip:remote_folder
# 下载单个文件
scp remote_username@remote_ip:remote_folder  local_folder
# 下载文件夹
scp -r remote_username@remote_ip:remote_folder  local_folder
```

**这个时候你的 `dist/` 文件夹应该已经在服务器的某处目录下了, 下面就是用 `Nginx` 启动它了.**

### 四、安装 Nginx

1. **安装Nginx, 先通过 `yum search nginx` 看看有无 Nginx 的源。如果成功则执行下列命令安装Nginx。**

```bash
sudo yum install -y nginx
```

2. **启动Nginx并设置开机自动运行**

```bash
#启动nginx服务
sudo systemctl start nginx.service		
#设置nginx为开机启动
sudo systemctl enable nginx.service		
```

3. **在 `ssh` 中输入**

```
curl 127.0.0.1	// 这个会直接访问到本地地址
```

如果结果显示:

```bash
curl: (7) Failed connect to 127.0.0.1:80; Connection refused
```

那说明你的 `Nginx` 出了点问题, 重新排查一下. 

如果命令行中打印出 `html` 的文档结构, 那么就说明 `Nginx` 安装成功可以进入下一步了.

### 五、配置 Nginx

输入命令行： `sudo vim /etc/nginx nginx.conf`, 进入 `Nginx` 配置文件.

进去以后会发现很多语句都用 `#` 给你注释掉了, 先忽略这些语句, 找到 `server` 配置的地方.

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200517022809.png)

`标注1` 是指监听的端口号为 `8089`, 意思就是你要输入这样的地址才可以访问到: 

```
http://你的ip地址:8089
```

`标注2` 则是指向你要启动的 `html` 文件所在的目录, 以及优先寻找的文件名.

详细的配置教程可以参考这篇文章:

- [《 Nginx安装及其配置详细教程 》](https://www.cnblogs.com/lywJ/p/10710361.html) 

### 六、 启动你的 Nginx

输入以下命令即可：

```
# 重新装载配置文件
sudo nginx -s reload
# 启动 Nginx
sudo nginx
```



---

*总结不易啊*

