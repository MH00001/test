# Git 常见命令使用大全

> 适合 Git 初学者查阅：包括版本上传、拉取、分支、版本回溯、冲突处理、远程仓库、常见报错等。  
> 本文以 Windows Git Bash 为例，路径示例使用 `/f/Javalearning/Test`。

---

## 目录

1. [Git 基本概念](#1-git-基本概念)
2. [进入 Git 仓库目录](#2-进入-git-仓库目录)
3. [查看状态和历史](#3-查看状态和历史)
4. [版本上传：add、commit、push](#4-版本上传addcommitpush)
5. [拉取远程代码：pull 和 fetch](#5-拉取远程代码pull-和-fetch)
6. [push 和 pull 的核心区别](#6-push-和-pull-的核心区别)
7. [分支常用命令](#7-分支常用命令)
8. [本地分支与远程分支的关系](#8-本地分支与远程分支的关系)
9. [版本回溯和撤销](#9-版本回溯和撤销)
10. [冲突处理](#10-冲突处理)
11. [stash 临时保存修改](#11-stash-临时保存修改)
12. [远程仓库相关命令](#12-远程仓库相关命令)
13. [常见报错解决](#13-常见报错解决)
14. [日常推荐工作流](#14-日常推荐工作流)
15. [常用命令速查表](#15-常用命令速查表)

---

# 1. Git 基本概念

Git 可以理解成一个“代码版本管理工具”。

它主要有几个区域：

```text
工作区          暂存区          本地仓库          远程仓库
Working Tree   Staging Area    Local Repo       Remote Repo
```

常见操作流程：

```bash
修改文件
git add 文件名
git commit -m "提交说明"
git push origin 分支名
```

含义：

```text
工作区修改  ->  git add  ->  暂存区
暂存区      ->  git commit  ->  本地仓库
本地仓库    ->  git push  ->  远程仓库 GitHub
```

---

# 2. 进入 Git 仓库目录

Git 命令必须在 Git 仓库目录中执行。

如果你的仓库在：

```bash
/f/Javalearning/Test
```

那么你要先进入：

```bash
cd /f/Javalearning/Test
```

如果你在上一级目录：

```bash
/f/Javalearning
```

执行 Git 命令，可能会报错：

```bash
fatal: not a git repository (or any of the parent directories): .git
```

意思是：当前目录不是 Git 仓库。

可以用下面命令确认当前目录：

```bash
pwd
```

查看当前目录下有哪些文件：

```bash
ls
```

---

# 3. 查看状态和历史

## 3.1 查看当前文件状态

```bash
git status
```

常见输出：

```text
On branch master
nothing to commit, working tree clean
```

表示当前在 `master` 分支，没有需要提交的修改。

如果看到：

```text
Untracked files:
  feature2.txt
```

表示这个文件是新文件，还没有被 Git 跟踪。

---

## 3.2 查看当前分支

```bash
git branch
```

示例：

```text
* feature2
  master
  new-feature
```

带 `*` 的是当前所在分支。

---

## 3.3 查看提交历史

```bash
git log
```

更推荐简洁版：

```bash
git log --oneline
```

示例：

```text
65357c8 add777
e085302 add test01.md
c25cbb7 master
7294a3c Test.md
```

每一行前面的短编号就是提交 ID。

---

## 3.4 查看修改内容

查看工作区文件修改了什么：

```bash
git diff
```

查看已经 `git add` 之后的暂存区差异：

```bash
git diff --staged
```

---

# 4. 版本上传：add、commit、push

## 4.1 添加单个文件到暂存区

```bash
git add test01.md
```

意思是：把 `test01.md` 的修改加入暂存区，准备提交。

---

## 4.2 添加所有修改到暂存区

```bash
git add .
```

意思是：把当前目录下所有修改、新增、删除都加入暂存区。

注意：初学阶段使用 `git add .` 前建议先执行：

```bash
git status
```

确认哪些文件会被提交。

---

## 4.3 提交到本地仓库

```bash
git commit -m "add test01.md"
```

含义：把暂存区的内容提交到本地仓库，提交说明是 `add test01.md`。

示例：

```bash
git add test01.md
git commit -m "add test01.md"
```

---

## 4.4 推送到远程仓库

```bash
git push origin master
```

重点理解：

```bash
git push 远程仓库名 本地分支名
```

所以：

```bash
git push origin master
```

默认等价于：

```bash
git push origin master:master
```

含义：

```text
把本地 master 分支 推送到 远程 origin 的 master 分支
```

---

## 4.5 推送本地分支到远程不同名分支

格式：

```bash
git push origin 本地分支名:远程分支名
```

例如：

```bash
git push origin feature2:new-feature
```

含义：

```text
把本地 feature2 分支 推送到 远程 new-feature 分支
```

再比如：

```bash
git push origin master:new-feature
```

含义：

```text
把本地 master 分支 推送到 远程 new-feature 分支
```

---

## 4.6 推送当前分支到远程指定分支

```bash
git push origin HEAD:new-feature
```

`HEAD` 表示当前分支的最新提交。

如果你当前在 `feature2` 分支，那么：

```bash
git push origin HEAD:new-feature
```

等价于：

```bash
git push origin feature2:new-feature
```

---

## 4.7 第一次推送并建立追踪关系

```bash
git push -u origin feature2
```

`-u` 的意思是建立本地 `feature2` 和远程 `origin/feature2` 的默认关联。

建立后，以后在 `feature2` 分支上可以直接：

```bash
git push
```

不用每次写：

```bash
git push origin feature2
```

---

# 5. 拉取远程代码：pull 和 fetch

## 5.1 拉取远程分支到当前本地分支

```bash
git pull origin new-feature
```

重点理解：

```bash
git pull 远程仓库名 远程分支名
```

它的意思是：

```text
把远程 origin 的 new-feature 分支 拉取并合并到 当前所在的本地分支
```

注意：`pull` 的目标是当前本地分支，不是自动拉到本地同名分支。

例如当前在 `feature2`：

```bash
git branch
```

显示：

```text
* feature2
  master
```

执行：

```bash
git pull origin new-feature
```

含义是：

```text
origin/new-feature  --->  本地当前 feature2
```

如果当前在 `master`，同样执行：

```bash
git pull origin new-feature
```

含义就变成：

```text
origin/new-feature  --->  本地当前 master
```

---

## 5.2 使用 rebase 方式拉取

```bash
git pull --rebase origin master
```

含义：先把远程 `master` 拉下来，再把你本地的提交“接”到远程最新提交后面。

常见场景：推送时报错 `non-fast-forward`，可以先执行：

```bash
git pull --rebase origin master
git push origin master
```

优点：提交历史比较直，不会多出一个 merge commit。

---

## 5.3 fetch 只下载，不自动合并

```bash
git fetch origin
```

含义：从远程下载最新信息，但不改变你当前分支的代码。

查看远程分支：

```bash
git branch -r
```

示例输出：

```text
origin/master
origin/new-feature
```

如果你只想先看看远程有没有变化，可以用：

```bash
git fetch origin
git log --oneline --all --graph
```

---

# 6. push 和 pull 的核心区别

这是初学者最容易混淆的地方。

## 6.1 push

```bash
git push origin new-feature
```

默认等价于：

```bash
git push origin new-feature:new-feature
```

含义：

```text
本地 new-feature  --->  远程 new-feature
```

它看的是你命令里写的本地分支名，不一定是当前分支。

即使你当前在 `feature2`，执行：

```bash
git push origin new-feature
```

推送的也是：

```text
本地 new-feature  --->  远程 new-feature
```

不是：

```text
当前 feature2  --->  远程 new-feature
```

---

## 6.2 pull

```bash
git pull origin new-feature
```

含义：

```text
远程 new-feature  --->  当前本地分支
```

`pull` 非常看你当前在哪个分支。

你在哪个本地分支执行，它就拉到哪个本地分支。

---

## 6.3 对比总结

| 命令 | 分支名主要表示什么 | 目标是谁 |
|---|---|---|
| `git push origin master` | 本地 `master` | 推到远程 `master` |
| `git pull origin master` | 远程 `master` | 拉到当前本地分支 |
| `git push origin feature2:new-feature` | 本地 `feature2` 到远程 `new-feature` | 远程 `new-feature` |
| `git pull origin new-feature` | 远程 `new-feature` | 当前本地分支 |

---

# 7. 分支常用命令

## 7.1 查看本地分支

```bash
git branch
```

---

## 7.2 查看远程分支

```bash
git branch -r
```

---

## 7.3 查看所有分支

```bash
git branch -a
```

---

## 7.4 创建本地分支

```bash
git branch feature2
```

这只创建分支，不会切换过去。

---

## 7.5 创建并切换分支

推荐：

```bash
git switch -c feature2
```

旧写法：

```bash
git checkout -b feature2
```

---

## 7.6 切换分支

推荐：

```bash
git switch master
```

旧写法：

```bash
git checkout master
```

---

## 7.7 删除本地分支

安全删除：

```bash
git branch -d feature2
```

强制删除：

```bash
git branch -D feature2
```

---

## 7.8 删除远程分支

```bash
git push origin --delete new-feature
```

含义：删除远程 `origin` 上的 `new-feature` 分支。

---

# 8. 本地分支与远程分支的关系

## 8.1 建立本地分支追踪远程分支

如果远程有 `new-feature`，你想在本地创建同名分支并关联：

```bash
git switch -c new-feature origin/new-feature
```

或者：

```bash
git checkout -b new-feature origin/new-feature
```

---

## 8.2 查看本地分支追踪关系

```bash
git branch -vv
```

示例：

```text
* feature2    3b32b68 [origin/new-feature: ahead 1] addfeature2txt
  master      e085302 [origin/master] add test01.md
```

含义：

- `ahead 1`：本地比远程多 1 个提交。
- `behind 1`：本地比远程少 1 个提交。
- `ahead 1, behind 1`：本地和远程分叉了。

---

## 8.3 设置当前分支的上游分支

```bash
git branch --set-upstream-to=origin/new-feature feature2
```

含义：让本地 `feature2` 默认追踪远程 `origin/new-feature`。

设置之后，在 `feature2` 分支可以直接：

```bash
git pull
git push
```

---

# 9. 版本回溯和撤销

## 9.1 撤销工作区未提交的修改

撤销单个文件：

```bash
git restore test01.md
```

撤销所有未提交修改：

```bash
git restore .
```

注意：这会丢掉没有提交的修改。

---

## 9.2 已经 git add，但还没 commit

取消暂存单个文件：

```bash
git restore --staged test01.md
```

取消全部暂存：

```bash
git restore --staged .
```

取消暂存后，文件修改还在工作区。

---

## 9.3 撤销最近一次提交，但保留代码

```bash
git reset --soft HEAD~1
```

含义：

```text
撤销最近一次 commit
保留代码修改
保留暂存状态
```

适合：提交说明写错了，或者想重新提交。

---

## 9.4 撤销最近一次提交，代码保留但取消暂存

```bash
git reset --mixed HEAD~1
```

或者简写：

```bash
git reset HEAD~1
```

含义：

```text
撤销最近一次 commit
保留代码修改
取消暂存
```

---

## 9.5 撤销最近一次提交，并丢掉代码

```bash
git reset --hard HEAD~1
```

含义：

```text
撤销最近一次 commit
代码也回到上一个版本
当前修改会丢失
```

危险：没有提交的修改会被清掉。

---

## 9.6 回退到指定提交

先查看提交记录：

```bash
git log --oneline
```

示例：

```text
65357c8 add777
e085302 add test01.md
c25cbb7 master
```

回退到 `e085302`，并保留后续改动：

```bash
git reset --soft e085302
```

回退到 `e085302`，并丢弃后续改动：

```bash
git reset --hard e085302
```

---

## 9.7 已经推送到远程，安全撤销用 revert

如果提交已经推送到 GitHub，尤其多人协作，不推荐随便 `reset --hard` 后强推。

更安全的方式：

```bash
git revert 提交ID
```

例如：

```bash
git revert 65357c8
```

含义：创建一个新的提交，用来抵消 `65357c8` 的修改。

优点：不会破坏远程历史。

---

## 9.8 reset 和 revert 的区别

| 命令 | 是否改历史 | 是否适合已 push | 说明 |
|---|---|---|---|
| `git reset --soft` | 会 | 谨慎 | 回退提交，保留代码 |
| `git reset --hard` | 会 | 不推荐 | 回退提交，丢弃代码 |
| `git revert` | 不会 | 推荐 | 新建一个反向提交 |

---

## 9.9 后悔 reset 了怎么办：reflog

Git 会记录你 HEAD 移动过的位置。

查看：

```bash
git reflog
```

示例：

```text
65357c8 HEAD@{0}: reset: moving to HEAD~1
bacc19e HEAD@{1}: commit: add666
```

恢复到某个历史位置：

```bash
git reset --hard bacc19e
```

---

# 10. 冲突处理

## 10.1 什么时候会冲突

例如：

你本地修改了 `test01.md`：

```text
add777
```

远程也修改了同一个位置：

```text
add666
```

你执行：

```bash
git pull --rebase origin master
```

可能出现：

```text
CONFLICT (content): Merge conflict in test01.md
error: could not apply ...
```

---

## 10.2 查看冲突文件

```bash
git status
```

会看到类似：

```text
both modified: test01.md
```

打开文件：

```bash
code test01.md
```

你会看到：

```text
<<<<<<< HEAD
远程内容 add666
=======
本地内容 add777
>>>>>>> 65357c8
```

---

## 10.3 手动解决冲突

你要把文件改成最终想要的样子。

例如只保留：

```text
add777
```

保存文件后执行：

```bash
git add test01.md
```

如果你是在 rebase 中：

```bash
git rebase --continue
```

如果你是在 merge 中：

```bash
git commit
```

---

## 10.4 放弃 rebase

如果你不想继续当前 rebase，可以执行：

```bash
git rebase --abort
```

当终端显示类似：

```text
(master|REBASE 1/1)
```

说明正在 rebase 中。

此时不要直接 push，要先：

```bash
git rebase --continue
```

或者：

```bash
git rebase --abort
```

---

## 10.5 跳过当前提交

```bash
git rebase --skip
```

含义：跳过当前正在应用的提交。

谨慎使用，因为可能会丢掉当前提交的修改。

---

# 11. stash 临时保存修改

有时候你正在改代码，但还不想提交，又需要切换分支或 pull。

## 11.1 临时保存当前修改

```bash
git stash
```

保存时带说明：

```bash
git stash push -m "临时保存登录功能修改"
```

---

## 11.2 查看 stash 列表

```bash
git stash list
```

示例：

```text
stash@{0}: On feature2: 临时保存登录功能修改
```

---

## 11.3 恢复最近一次 stash

恢复并保留 stash 记录：

```bash
git stash apply
```

恢复并删除 stash 记录：

```bash
git stash pop
```

---

## 11.4 删除 stash

删除最近一次：

```bash
git stash drop
```

清空所有 stash：

```bash
git stash clear
```

---

# 12. 远程仓库相关命令

## 12.1 查看远程仓库地址

```bash
git remote -v
```

示例：

```text
origin  https://github.com/MH00001/test.git (fetch)
origin  https://github.com/MH00001/test.git (push)
```

---

## 12.2 添加远程仓库

```bash
git remote add origin https://github.com/用户名/仓库名.git
```

例如：

```bash
git remote add origin https://github.com/MH00001/test.git
```

---

## 12.3 修改远程仓库地址

```bash
git remote set-url origin https://github.com/用户名/新仓库名.git
```

---

## 12.4 删除远程仓库关联

```bash
git remote remove origin
```

---

## 12.5 克隆远程仓库

```bash
git clone https://github.com/用户名/仓库名.git
```

例如：

```bash
git clone https://github.com/MH00001/test.git
```

克隆后进入目录：

```bash
cd test
```

---

# 13. 常见报错解决

## 13.1 fatal: not a git repository

报错：

```text
fatal: not a git repository (or any of the parent directories): .git
```

原因：你当前目录不是 Git 仓库。

解决：

```bash
cd /f/Javalearning/Test
```

确认：

```bash
ls
```

如果这个目录下有 `.git`，才是仓库。

Git Bash 默认不显示隐藏文件，可以用：

```bash
ls -a
```

---

## 13.2 non-fast-forward

报错：

```text
! [rejected] master -> master (non-fast-forward)
```

原因：远程分支有本地没有的提交，Git 不允许你直接覆盖远程。

安全解决方式：

```bash
git pull --rebase origin master
git push origin master
```

如果你推的是本地 `feature2` 到远程 `new-feature`：

```bash
git pull --rebase origin new-feature
git push origin feature2:new-feature
```

注意：执行 `pull --rebase origin new-feature` 前，要确认你当前就在 `feature2` 分支。

---

## 13.3 强制推送覆盖远程

如果你明确知道要覆盖远程，可以用：

```bash
git push --force-with-lease origin master
```

或指定本地到远程：

```bash
git push --force-with-lease origin feature2:new-feature
```

比 `--force` 更安全，因为它会检查远程是否被别人更新过。

危险写法：

```bash
git push --force origin master
```

一般不推荐，除非你非常确定。

---

## 13.4 当前处于 rebase 状态

终端显示：

```text
(master|REBASE 1/1)
```

说明你正在 rebase 中。

解决完冲突后继续：

```bash
git add 冲突文件
git rebase --continue
```

放弃本次 rebase：

```bash
git rebase --abort
```

不要在 rebase 未完成时直接 `git push`。

---

## 13.5 GitHub 连接失败

报错：

```text
Failed to connect to github.com:443
Could not connect to server
```

或：

```text
Recv failure: Connection was reset
```

这通常是网络问题，不是 Git 分支问题。

可以测试：

```bash
ping github.com
```

但 `ping` 通不代表 HTTPS 一定通。

继续测试：

```bash
curl -I https://github.com
```

如果失败，可以尝试：

```bash
ipconfig /flushdns
```

检查代理：

```bash
git config --global --get http.proxy
git config --global --get https.proxy
```

如果输出了错误代理，可以清除：

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

---

# 14. 日常推荐工作流

## 14.1 单人开发 master 分支

```bash
cd /f/Javalearning/Test
git pull --rebase origin master
git status
git add .
git commit -m "提交说明"
git push origin master
```

---

## 14.2 在 feature2 分支开发，并推送到远程 new-feature

先切换分支：

```bash
git switch feature2
```

拉取远程 `new-feature` 到当前 `feature2`：

```bash
git pull --rebase origin new-feature
```

修改代码后：

```bash
git add .
git commit -m "add feature2 changes"
```

推送本地 `feature2` 到远程 `new-feature`：

```bash
git push origin feature2:new-feature
```

---

## 14.3 新建本地分支并推送到远程新分支

```bash
git switch -c add777-branch
git add .
git commit -m "add777"
git push -u origin add777-branch
```

这样远程会出现一个新分支：

```text
origin/add777-branch
```

---

## 14.4 不想要最近一次提交

保留代码：

```bash
git reset --soft HEAD~1
```

丢掉代码：

```bash
git reset --hard HEAD~1
```

如果已经推送到远程，推荐：

```bash
git revert 提交ID
git push origin 分支名
```

---

# 15. 常用命令速查表

## 15.1 基础命令

| 命令 | 作用 |
|---|---|
| `pwd` | 查看当前路径 |
| `ls` | 查看当前目录文件 |
| `ls -a` | 查看隐藏文件 |
| `cd 路径` | 进入目录 |
| `git status` | 查看 Git 状态 |
| `git log --oneline` | 查看简洁提交历史 |
| `git branch` | 查看本地分支 |
| `git branch -a` | 查看所有分支 |

---

## 15.2 提交和上传

| 命令 | 作用 |
|---|---|
| `git add 文件名` | 添加单个文件到暂存区 |
| `git add .` | 添加所有修改到暂存区 |
| `git commit -m "说明"` | 提交到本地仓库 |
| `git push origin master` | 推送本地 master 到远程 master |
| `git push origin feature2:new-feature` | 推送本地 feature2 到远程 new-feature |
| `git push -u origin 分支名` | 第一次推送并建立追踪关系 |

---

## 15.3 拉取和同步

| 命令 | 作用 |
|---|---|
| `git pull origin master` | 拉取远程 master 到当前本地分支 |
| `git pull --rebase origin master` | 以 rebase 方式拉取远程 master |
| `git fetch origin` | 只下载远程信息，不合并 |
| `git branch -r` | 查看远程分支 |

---

## 15.4 分支

| 命令 | 作用 |
|---|---|
| `git switch 分支名` | 切换分支 |
| `git switch -c 分支名` | 创建并切换分支 |
| `git checkout -b 分支名` | 创建并切换分支，旧写法 |
| `git branch -d 分支名` | 删除本地分支 |
| `git push origin --delete 分支名` | 删除远程分支 |
| `git branch -vv` | 查看分支追踪关系 |

---

## 15.5 撤销和回退

| 命令 | 作用 |
|---|---|
| `git restore 文件名` | 撤销工作区某文件修改 |
| `git restore .` | 撤销所有工作区修改 |
| `git restore --staged 文件名` | 取消暂存某文件 |
| `git reset --soft HEAD~1` | 撤销最近提交，保留代码和暂存 |
| `git reset HEAD~1` | 撤销最近提交，保留代码但取消暂存 |
| `git reset --hard HEAD~1` | 撤销最近提交并丢弃代码 |
| `git revert 提交ID` | 安全撤销某次已提交内容 |
| `git reflog` | 查看 HEAD 历史移动记录 |

---

## 15.6 冲突和 rebase

| 命令 | 作用 |
|---|---|
| `git rebase --continue` | 解决冲突后继续 rebase |
| `git rebase --abort` | 放弃当前 rebase |
| `git rebase --skip` | 跳过当前提交 |
| `git add 冲突文件` | 标记冲突已解决 |

---

## 15.7 stash

| 命令 | 作用 |
|---|---|
| `git stash` | 临时保存当前修改 |
| `git stash list` | 查看 stash 列表 |
| `git stash apply` | 恢复 stash，保留记录 |
| `git stash pop` | 恢复 stash，并删除记录 |
| `git stash drop` | 删除最近一次 stash |
| `git stash clear` | 清空所有 stash |

---

# 最重要的几句话

## push 的重点

```bash
git push origin 分支名
```

这里的“分支名”默认是本地分支名，会推送到远程同名分支。

```bash
git push origin feature2:new-feature
```

表示：

```text
本地 feature2  --->  远程 new-feature
```

---

## pull 的重点

```bash
git pull origin 分支名
```

这里的“分支名”是远程分支名，会被拉到当前本地分支。

```bash
git pull origin new-feature
```

如果你当前在 `feature2`，表示：

```text
远程 new-feature  --->  当前本地 feature2
```

---

## non-fast-forward 的重点

出现：

```text
non-fast-forward
```

通常表示：

```text
远程分支有本地没有的提交
```

安全处理：

```bash
git pull --rebase origin 远程分支名
git push origin 本地分支名:远程分支名
```

强制覆盖远程：

```bash
git push --force-with-lease origin 本地分支名:远程分支名
```

---

## rebase 状态的重点

终端显示：

```text
(master|REBASE 1/1)
```

说明 rebase 没完成。

继续：

```bash
git add 冲突文件
git rebase --continue
```

放弃：

```bash
git rebase --abort
```

---

# 推荐你现在记住的 5 个命令组合

## 查看当前状态

```bash
git status
git branch
git log --oneline
```

## 正常提交并推送当前 master

```bash
git add .
git commit -m "提交说明"
git push origin master
```

## 本地 feature2 推到远程 new-feature

```bash
git push origin feature2:new-feature
```

## 远程 new-feature 拉到当前 feature2

```bash
git switch feature2
git pull --rebase origin new-feature
```

## 回退最近一次提交

```bash
git reset --soft HEAD~1
```

或危险丢弃：

```bash
git reset --hard HEAD~1
```
