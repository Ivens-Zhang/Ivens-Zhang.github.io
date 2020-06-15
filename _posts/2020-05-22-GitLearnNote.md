# Git 学习笔记

- [《 Git最新教程通俗易懂 》](https://www.bilibili.com/video/BV1FE411P7B3/?p=3&t=82)
- [《 视频同步笔记 》](https://mp.weixin.qq.com/s/Bf7uVhGiu47uOELjmC5uXQ)
- [《 Git入门 》](https://www.imooc.com/learn/1052)
- [《 史上最浅显易懂的Git教程 》](https://www.liaoxuefeng.com/wiki/896043488029600)

## 一、简介

### 1.1. 版本控制的两大类型

#### 集中版本控制  SVN

所有的版本数据都`保存在服务器上`，协同开发者从服务器上同步更新或上传自己的修改。

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200522174557.png" style="zoom:67%;" />

所有的版本数据都存在服务器上，用户的本地只有自己以前所同步的版本，`如果不连网的话，用户就看不到历史版本，也无法切换版本验证问题，或在不同分支工作。`而且，所有数据都保存在单一的服务器上，有很大的风险这个服务器会损坏，这样就会丢失所有的数据，当然可以定期备份。

代表产品：

- SVN
- CVS
- VSS

#### 分布式版本控制 Git

每个人都拥有全部的代码！安全隐患！（代码泄露的风险）

所有版本信息仓库全部同步到本地的每个用户，这样就可以在本地查看所有版本历史，可以离线在本地提交，只需在连网时 push 到相应的服务器或其他用户那里。由于每个用户那里保存的都是所有的版本数据，只要有一个用户的设备没有问题就可以恢复所有的数据，但这增加了本地存储空间的占用。

不会因为服务器损坏或者网络问题，造成不能工作的情况！

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200522175449.png" style="zoom:67%;" />

### 1.2. Git 的历史

同生活中的许多伟大事物一样，`Git` 诞生于一个极富纷争大举创新的年代。

Linux 内核开源项目有着为数众广的参与者。绝大多数的 `Linux` 内核维护工作都花在了提交补丁和保存归档的繁琐事务上 (1991－2002 年间)。到 2002 年，整个项目组开始启用一个专有的分布式版本控制系统 BitKeeper 来管理和维护代码。

Linux 社区中存在很多的大佬！破解研究 `BitKeeper `！

到了 2005 年，开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了 Linux 内核社区免费使用 BitKeeper 的权力。这就迫使 Linux 开源社区 (特别是 Linux 的缔造者 Linus Torvalds) 基于使用 BitKeeper 时的经验教训，开发出自己的版本系统。（2 周左右！） 也就是后来的 Git！

Git 是免费、开源的，最初 Git 是为辅助 Linux 内核开发的，来替代 BitKeeper！



## 二、Git 基础

### 2.1. 常用 Linux 命令

- pwd : 显示当前所在的目录路径。
- mv 移动文件, mv index.html src index.html 是我们要移动的文件, src 是目标文件夹, 当然, 这样写, 必须保证文件和目标文件夹在同一目录下。
- `# `表示注释

---

*这里只列出了我需要补充的命令*

### 2.2. 四个工作区

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200523112306.png" style="zoom:80%;" />

- Workspace：工作区，就是你平时存放项目代码的地方
- Index / Stage：暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息
- Repository：仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本
- Remote：远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换



### 2.3. 本地项目结构

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200523113636.png" style="zoom:67%;" />

- Directory：使用Git管理的一个目录，也就是一个仓库，包含我们的工作空间和Git的管理空间。
- WorkSpace：需要通过Git进行版本控制的目录和文件，这些目录和文件组成了工作空间。
- .git：存放Git管理信息的目录，初始化仓库的时候自动创建。
- Index/Stage：暂存区，或者叫待提交更新区，在提交进入repo之前，我们可以把所有的更新放在暂存区。
- Local Repo：本地仓库，一个存放在本地的版本库；HEAD会只是当前的开发分支（branch）。
- Stash：隐藏，是一个工作状态保存栈，用于保存/恢复WorkSpace中的临时状态。



### 2.4. 专用名词

- Fetch（获取），从远程代码库更新数据到本地代码库。 `注意` ：Fetch 只是将代码更新到本地代码库，你需要检出（check out）或与当前工作分支合并（merge）才能在你的工作目录中看到代码的改变。
- HEAD，指向你正在工作中的本地分支的指针
- Master 分支：主分支，所有提供给用户使用的正式版本，都在这个主分支上发布。 [关于分支管理的扩展阅读](http://www.ruanyifeng.com/blog/2012/07/git.html)
- Tags（标签）：用来记录重要的版本历史，例如里程碑版本
- Origin：默认的 remote的名称

## 三、分支管理

`HEAD`指向当前所处的位置，如果只有一条分支，那 `HEAD` 则指向 `master`。`Git` 创建分支的机制也很简单，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200524221719.png)

现在，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200524222058.png)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200524222127.png)

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20200524222152.png)

要记住的命令：

```bash
# 加上-b参数表示创建并切换
git checkout -b 分支名

# 切换分支
git checkout <name>或者git switch <name>

# 用于合并指定分支到当前分支
git merge 分支名

# 删除分支
git branch -d 分支名
```

### 3.1. 分支冲突

- [《 解决冲突 》](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)

当我们合并分支时遇到冲突了，先输入`git status` 查看下哪个文件冲突了：

```bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

可以看出，是我们库中的 `readme.md` 文件冲突了，这时我们输入 `vim ./readme.md` ，可以看到：

```bash
Hello, This is git train's README.md.
this is a update for diff.
adddddddddddddddddddddddddddddddddddddddddd.

<<<<<<< HEAD
现在是 master 分支的内容了！
=======
现在是 dev1 分支的内容了！
>>>>>>> dev
```

还是依照 `master` 吧，那就把其他多余的内容删去。

```bash
Hello, This is git train's README.md.
this is a update for diff.
adddddddddddddddddddddddddddddddddddddddddd.
现在是 master 分支的内容了！
```

之后，重新 `add` 以及 `commit` 一次就好了，我们可以看到图形化的提交线路：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
*   9b9dda1 (HEAD -> master) conflict fixed
|\
| * c563002 (dev) update readme.md
* | aab40e1 master update
|/
* c2db8ac dev branch update
* d0ae783 add wrong word.
* cf05013 add some message
* 1dd2ab8 update readme.md
```



### 3.2. 禁用 `Fast Forward` 模式

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

可能文字不够直白，这是没有禁用`Fast forward`模式的路线图：

```bash
$ git log --graph --pretty=oneline
* 3479d9d82e1279272160d49ffe2df8d27f68ac24 (HEAD -> master, ivens) update readme
*   393277834e8bf6e55be8815823070c8ba8874f6b merge with no-ff
|\
| * f1332b7000f68e4cf661887cd0a5bb407251083e add new msg
|/
*   9b9dda17bb0c42b7d707776ad5dfd06e9221a724 conflict fixed
|\
| * c5630022af63646d81752e76fb86dff8bdf46502 update readme.md
* | aab40e1403392e78c094132cde02fd047937497b master update
|/
* c2db8ac519baf8c41822e9cd40acdc3644f8d515 dev branch update
* d0ae7830ae2bc687a32459fc09fa7f8fcfac65fd add wrong word.
* cf05013850f7bd290387fcde54675123ab3e9b5e add some message
* 1dd2ab855c8fe24383ea03783df6833154a30403 update readme.md
```

这是禁用了禁用`Fast forward`模式的路线图：

```bas
$ git log --graph --pretty=oneline
*   393277834e8bf6e55be8815823070c8ba8874f6b (HEAD -> master) merge with no-ff
|\
| * f1332b7000f68e4cf661887cd0a5bb407251083e (dev) add new msg
|/
*   9b9dda17bb0c42b7d707776ad5dfd06e9221a724 conflict fixed
|\
| * c5630022af63646d81752e76fb86dff8bdf46502 update readme.md
* | aab40e1403392e78c094132cde02fd047937497b master update
|/
* c2db8ac519baf8c41822e9cd40acdc3644f8d515 dev branch update
* d0ae7830ae2bc687a32459fc09fa7f8fcfac65fd add wrong word.
* cf05013850f7bd290387fcde54675123ab3e9b5e add some message
* 1dd2ab855c8fe24383ea03783df6833154a30403 update readme.md
```

可以看到，被合并的分支在 `merge` 操作后并不会被带到最新的节点上，而是会留在被合并之前的位置。



### 3.3. `stash` 储藏更改

当你在开发分支中已经暂存了许多更改后，突然说 `master` 分支下出现了一个 bug，需要尽快修复，考虑到目前分支上的暂存还没有提交，应该输入以下指令进行储藏：

- `git stash`

然后我们看看工作区：

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

发现工作区已经干净如新，那么现在可以开始修复这个bug了：

1. `git switch master` &  `git branch -c bug-001`
2. a few moment later
3. `git switch master`
4. `git merge bug-001` & `git branch -d bug-001`
5. `git switch dev`

这时怎么恢复之前被我们储存的文件呢？

- `git stash list` 查看存储列表
- `git stash pop` 恢复同时把列表中对应条目删除



*这个功能应用场景应该为：临时需要切换到其他分支，并且目前分支上的更改只暂存并未提交。*





## 标签管理

常用指令：

- 查看所有标签			`git tag`
- 创建标签                  `git tag <tag-name>`
- 删除标签                  `git tag -d <tag-name>`
- 推送标签                  `git push origin <tag-name>`

















