# Golden Job 构建流程

> **写给谁看：**
>
> - 项目运维人员
> - 项目开发人员
> - 平台管理员

## 业务流程

### 简介

**`人工智能实训平台`** 是一个基于 Microsoft 开源的 pai 项目，以此作为资源调度工具，在其上封装了一些常用的 `to B` 服务，以此完成一个面向机构的人工智能实训平台。 

### 核心功能

- 为普通用户提供一个可以一键启动的 AI 开发环境。

### 优势

- 与 Microsoft 的 pai 并非强耦合关系
- 对 Azure 云服务不是强需求，可以用其他公有云、私有云、混合云、物理机替代
- 一键启动环境，确实方便多了

### 项目架构图

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200703180457.png)

### 技术栈选型

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200703191900.png)

## 名词解释

> 每一个学生创建环境后，open pai 平台会创建对应的 Job，这个 Job 对应的则是一个 docker container，而每个 Job 都是互相独立的，他们从哪里来的呢？项目中设置了一种 Golden Job，可以理解为普通 Job 的模板。而这每一个 Golden Job 则对应了一个包含了特殊数据集，课件，环境的 docker image。

### 1. Job：

`Platform for AI` 中的任务，对应实训平台课程中的**`环境`**。

### 2. Golden Job:

`Platform for AI` 中用户环境创建的 Job 的模板。

### 3. Docker Image

`Golden Job` 的数据集，课件，环境的镜像。



## 操作步骤

### 1、 找到需要替换的文件

下文以修改 `Jupyter Notebook & VSCode` 中的主题 logo 举例。

首先我们进入 `Platform for AI` 中，找到一个正在 `Running` 的 `Golden Job`，点击进入详情。

 ![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200703213805.png)

我们可以看到有一个 `IP` 和 一个 `Port`， 我们把这两个值输入到地址栏，如果这个 `Job` 是正在进行中的，那么我们可以看到如下的页面。

![image-20200703214008948](C:\Users\zhangyi\AppData\Roaming\Typora\typora-user-images\image-20200703214008948.png)

可能之前用过 Jupyter Notebook 的人，看到这个就会觉得还挺亲切的了。

那么现在我们先找一下对应的这个 logo，存放在什么路径中，做一个同名替换即可。

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200703214155.png)

就是如下路径，我们直接在这里替换，这只会是一次性的，下次当我们重置环境后，这些改动又会消失，注意，我们现在是在一个普通的 `Job` 中，要永久性地改动，那就要改动 `Job` 的爸爸—— `Golden job`, 然而 `Golden Job` 要想让他也持久化，那还是得找到根源——`docker image`。



### 2、修改 DockFile & 打包

由于在此之前我们将 `docker image` 相关的文件夹利用 `git` 初始化了，所以找个服务器，`clone` 一下，这是文件目录。

```bash
[shaiic@vm01 openpai]$ tree
├── build.sh																	 # 这是一键 build + push 脚本
├── dockerfiles																 # 这里存放几个 docker image 的 DockerFile
│   ├── OOB1
│   │   ├── Dockerfile.bartu
│   │   ├── readme.txt
│   │   └── requirements.bartu.txt
│   ├── OOB2
│   │   └── Dockerfile.miniailab
│   ├── OOB3
│   │   └── Dockerfile.aiedu
│   └── OOB4
│       ├── Dockerfile.sjtu
│       ├── readme.txt
│       └── requirements.txt
├── README.md
└── yamls																				# 这个文件夹是用来存放 Golden Job 的配置文件
    ├── old
    │   ├── GoldenJupyterJobTemplate.yaml
    │   └── GolderJupyterReal_NotAdmin.yaml
    ├── OOB1-pythonBartu.yaml
    ├── OOB2-miniailab.yaml
    ├── OOB3-aiedu.yaml
    └── OOB4-tf.keras.gpu.yaml
```

哪里是我们要修改的地方呢？

- **`dockerfiles/`**

本项目所有的 docker image 都存放在这个目录下，以 `OOB1` 举例，其中核心是 `Dockerfile.bartu`， 打开看看里面什么样。

```bash
$ cat dockerfiles/OOB1/Dockerfile.bartu

# the base image
#FROM openpai/standard:python_3.6-tensorflow_2.0.0-cpu
FROM shaiic/standard:ddebby.ai.v0.1.0

ADD requirements.bartu.txt /

# to support nfs share
RUN apt-get update
RUN apt-get install nfs-common -y

# install tensorflow packages we specified into the base environment
RUN pip install --trusted-host pypi.python.org -r requirements.bartu.txt

# download the coder binary, untar it, and allow it to be executed
RUN git clone https://github.com/bartuer/training_notebook.git /root/courses

# ==========================================================================
# 以上是一个 AI 学习课程的环境安装，从这里开始 定制logo的替换，分为两步：
# 	1. 删除之前的 logo 原文件
#		2. wget 下载自定义文件 & 重命名为 logo.png 或 favicon.ico 等

# remove logo and icons
RUN rm -f /opt/conda/lib/python3.7/site-packages/notebook/static/base/images/logo.png /opt/conda/lib/python3.7/site-packages/notebook/static/base/images/favicon.ico /opt/conda/lib/python3.7/site-packages/notebook/static/base/images/favicon-terminal.ico

# download images
RUN wget https://shaiictestblob01.blob.core.chinacloudapi.cn/share-all/logo.png -O /opt/conda/lib/python3.7/site-packages/notebook/static/base/images/logo.png
RUN wget https://shaiictestblob01.blob.core.chinacloudapi.cn/share-all/icon.png -O /opt/conda/lib/python3.7/site-packages/notebook/static/base/images/favicon.ico
RUN wget https://shaiictestblob01.blob.core.chinacloudapi.cn/share-all/icon.png -O /opt/conda/lib/python3.7/site-packages/notebook/static/base/images/favicon-terminal.ico
```

好了，按下 `Esc`， 输入 `:wq` 保存退出。

接下来是打包镜像了，输入以下命令：

```bash
docker build -f <要build的DockerFile文件名称> -t <Docker Hub用户名>/<image名>:<tag号> .
```

然后输入以下指令查看是否打包成功：

```bash
$ sudo docker images | grep <image名>
```

看看打印出的列表是否有你之前打包好的 `image`，如果有，那代表 build 成功了，现在就到下一步，把 image 推送到 `Docker Hub` 上。输入以下指令：

```bash
sudo docker login -u <你的Docker Hub用户名>
... input password ...
sudo docker push <Your Docker Hub Account>/<image name>:<tag name>
example: sudo docker push shaiic/ai-edu:v1.0
```

最后，登录 `Docker Hub` 查看一下有没有更新即可。





















