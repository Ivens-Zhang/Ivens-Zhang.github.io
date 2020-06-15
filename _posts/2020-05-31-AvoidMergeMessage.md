# 如何避免提交代码时出现“Merge branch 'dev' of xxx into dev”

### 根本原因：

当前本地仓库落后于远程仓库，并且本地以及有 commit 记录时，Git 会自动帮我们更新到最新。



### 打个比方：

A 修改了 index.js，commit 后 push 成功。

B 修改了 readme.md，commit 后 push 发现失败，然后 pull + push 成功。

这时，远程仓库的 commit 记录中就会出现一条，Merge branch 'dev' of http://10.0.0.7:8080/aichef/aichef-web into dev。

在进行 pull 操作的同时，其实就是 `fetch+merge` 的一个过程。我们从 `remote` 分支中拉取新的更新，然后再合并到本地分支中去。

1. 如果 remote 分支超前于本地分支，并且本地分支没有任何 commit 的，直接从 `remote` 进行 `pull` 操作，默认会采用 `fast-forward` 模式，这种模式下，并不会产生合并节点，也就是说不会产生多余的那条 log 信息
2. 如果像之前那样，本地先 commit 后再去 pull，那么此时，remote 分支和本地会分支会出现分叉，这个时候使用 pull 操作拉取更新时，就会进行分支合并，产生合并节点和 log 信息。



### 解决方法：

为了去除自动生成的 log 信息，有以下几种解决方案：

1. 如果你使用的是 `Git Bash`，直接使用 `git pull --rebase`。如果拉取不产生冲突，会直接 rebase，不会产生分支合并操作，如果有冲突则需要手动 `fix` 后，自行合并。
2. 如果忘记拉取时变基该怎么办，我们也可以 `git pull` 后，手动 `git rebase`，最后再提交上去。



### 参考文档：

- [Git push 时如何避免出现 "Merge branch 'master' of ..."](https://www.cnblogs.com/Sinte-Beuve/p/9195018.html)

- [git解决merge branch](https://blog.csdn.net/wm5920/article/details/79731983)

- [Rebase](https://www.liaoxuefeng.com/wiki/896043488029600/1216289527823648)