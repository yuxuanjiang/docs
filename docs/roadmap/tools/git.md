# Git



## Git, Github, GitLab 分别是什么？

Git 是一个版本控制的软件
GitHub 与 GitLab 都是用于管理版本的服务端软件
Github 提供免费服务（代码需公开）及付费服务 (代码为私有)
GitLab 用于在企业内部管理 Git 版本库，功能上类似于 Github

## 为什么要使用 Git

* 本地建立版本库
* 本地版本控制
* 多主机异地协同工作
* 重写提交说明
* 有后悔药可以吃
* 更好用的提交列表
* 更好的差异比较
* 更完善的分支系统
* 速度极快

## Git 工作模式

* 版本库初始化
    * 个人计算机从版本服务器同步
* 操作
    * 90%以上的操作在个人计算机上
    * 添加文件
    * 修改文件
    * 提交变更
    * 查看版本历史等
* 版本库同步
    * 将本地修改推送到版本服务器

## Git 基础

* 直接记录快照，而非差异比较
* 近乎所有操作都在本地执行
* 时刻保持数据完整性
* 多数操作仅添加数据
* 文件的三种状态
    * 已修改(modified)
    * 已暂存(staged)
    * 已提交(committed)

## Git 文件状态

* Git 文件
    * 已被版本库管理的文件
* 已修改
    * 在工作目录修改Git文件
* 已暂存
    * 对已修改的文件执行Git暂存操作，将文件存入暂存区
* 已修改
    * 将已暂存的文件执行Git提交操作，将文件存入版本库

![-w730](/Users/yuxuan/Desktop/🤔 笔记/JVM笔记/media/15900805379592.jpg)

## 本地版本库与服务器版本库之间的关系

![-w624](/Users/yuxuan/Desktop/🤔 笔记/JVM笔记/media/15900805965556.jpg)

## Git 常用命令

* 获取版本库
    * git init
        * 用于初始化一个空的git仓库，并生成一个 .git 文件夹。同时创建一个默认的分支(master branch)
    * git clone
        * 将远程版本库中的文件复制一份到本地
* 版本管理    
    * git add
        * 将当前的已修改的文件纳入到git的暂存区
    * git commit
        * git commit -am "comments" : 合并 `git add .` 和 `git commit -m '...'` 语句
        * 将暂存区的文件纳入到git版本库中
    * git rm 
        * 将暂存区中文件回退到已修改状态
    * git mv
        * 为文件重命名或者移动到指定的目录
* 查看信息
    * git help
        * 查看git的帮助信息
    * git status
        * 查看当前的git工作区处于一个什么样的状态
    * git log
        * 查看git的提交日志
        * git log -n : 查看n条最近的提交日志
        * git log --pretty=oneline : 使用行内模式显示历史提交的日志
    * git diff
        * 比较暂存区，版本库等区域中代码差别
* 远程协作
    * git pull
        * 将远程版本库中的文件拉取到本地
    * git push
        * 将本地版本内容推送到远程版本库中

## 使用git status

```bash
// 处于master分支
On branch master

// 还没有任何的提交
No commits yet

// 未追踪的文件：下列文件并未纳入到git的暂存区中
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	test.txt

nothing added to commit but untracked files present (use "git add" to track)
```

## 设置用户名以及邮箱地址

```bash
~/.gitconfig : git config --global (全局)
.git/config : git config --local (项目)优先级最高

// 示例

git config --global user.name "Your Name"
git config --global user.email "email@address.com"

git config --local user.name "Your Name"
git config --local user.email "email@address.com"
```

## 使用 git rm <file> 来删除和恢复文件

```git
// git rm <file> 删除了一个文件并将该文件纳入到暂存区(staged)中

// 旧版本的 git 指令
git reset HEAD <file> // 将文件从暂存区恢复到工作区
git checkout -- <file>  // 将文件恢复到未删除的状态

// 新版本的 git 指令
git restore --staged <file> // 将删除文件从站暂存区恢复到工作区
git restore <file> // 将文件恢复到未删除的状态
```

## rm <file> 来删除和恢复文件

如果使用系统提供过的 `rm <file>` 来删除一个文件，这时被删除的文件并没有被纳入暂存区中
如果想将文件恢复到未修改的状态，只需要直接使用git指令 `git restore <file>`

```git
// git 旧版本指令
git checkout -- <file>

// git 新版本指令
git restore <file>
```

## 修改上次提交的注释信息

```bash
git commit --amend -m 'commit message'
```

## .gitignore 配置文件

在 .gitignore 文件中添加想要让 git 忽视的文件或者目录

```bash
*.extension # 忽略所有 .extension 结尾的文件
!lib.extension # 除了 lib.extension 
/TODO # 仅仅忽略根目录下的 TODO 文件夹，不包括其他目录下的 TODO 文件夹
build/ # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽视 doc 目录下的 .txt 结尾的文件，但不包括子目录的 <file>.txt 文件 (doc/server/*.txt) 
doc/**/*.txt. # 会忽视 doc目录下所有层级的 .txt 结尾的文件
```

## git 分支

分支在创建时，文件都是一样的，但是不同的分支可以做不同的开发，在将来某个时间点可以进行合并。

```bash
git branch # 显示当前git工作区中所有的分支(* 开头的是当前所在分支)
git branch <branch> # 创建一个名为 <branch_name> 的分支
git branch -d <branch> # 删除合并过的或者无修改过文件的分支
git branch -D <branch> # 不管该分支的内容是否被合并，直接删除该分支 
git branch -v # 显示当前所有分支的最新提交信息

git checkout <branch> # 切换到<branch> 分支
git checkout -b <branch> # 创建分支同时切换到新创建的分支下

```

## 分支的合并与冲突

### 如果 master 分支并没有做任何的修改，但是其他想直接合并某个有新的提交的分支

git merge <branch> : 将 <branch> 上的修改合并都主分支上

### 如果 master 和其他分支都有做新的提交的情况下

先执行 git merge <branch> 
然后修改冲突文件的内容
调用 git add <file> 告诉 git 这个冲突文件已经手动解决了
最后调用 git commit -a 来解决合并的问题

## HEAD & master

HEAD 是一个指针，指向当前的分支（当前在哪个分支上，HEAD 就指向哪个分支）
master 也是一个指针, 指向提交（commit）
如果有多个分支, 就是在 master 的基础上，新建了一个名字叫 dev 的指针

![-w591](/Users/yuxuan/Desktop/🤔 笔记/JVM笔记/media/15902829634955.jpg)

当 dev 做了一次提交操作，dev 就指向了第四次提交，然而 master还是指向第三次提交

![-w767](/Users/yuxuan/Desktop/🤔 笔记/JVM笔记/media/15902831579460.jpg)

当 master 和 dev 两个分支实现合并(merge)后, 则 master 和 dev 都指向了第四次提交 (fast-forward 快进) 

![-w691](/Users/yuxuan/Desktop/🤔 笔记/JVM笔记/media/15902832666022.jpg)


## fast-forward 版本快进

 如果可能, 合并分支时Git会使用 fast-forward模式
 在这种模式下，删除分支时会丢掉分支信息
 合并时会加上 --no-ff 参数会禁用 fast-forward, 这样会多出一个 commit id
    

    `git merge --no-ff <branch>` : 触发一次新的提交而不是快进

## 使用图形化的方式查看提交历史

`git log --graph`   

![-w809](/Users/yuxuan/Desktop/🤔 笔记/JVM笔记/media/15902855261595.jpg)


## 版本回退

```bash
# 回退到上一个版本
git reset --hard HEAD^ 

# 回退到之前的第n个提交的版本
git reset --head HEAD~1

# 回退到指定的 commit_id 版本
git reset --head <commit_id>

# 查看历史所有的提交和回退信息(操作日志)
git reflog
```



## Checkout 进阶与 Stash

```bash
# 丢弃掉相对于工作区中最后一个添加文件内容所做的变更
git restore <file>

# 将之前添加到暂存区(stage, index) 的内容从暂存区移除到工作区
git restore --staged <file>

# 创建一个 test 分支并且立刻切换到它
git checkout -b test

# 为分支重命名
git branch -m <old_name> <new_name>

# 临时保存当前工作区下载正在进行的任务
git stash

# 列出当前临时保存的工作区下进行的任务
git stash list

# 恢复上一次临时保存的工作区的任务,并删除这一条状态
git stash pop

# 恢复上一次临时保存的工作区的任务，但是并不删除状态 && 配合  git stash drop stash@{0} 手动删除
git stash apply
git stash apply stash@{0}

# 查看某个文件被哪个用户修改过
git blame <file>
```



## 标签 与 diff

###  Git 标签

* 新建标签，标签有两种： 轻量级标签 (lightweight) 与带有附注标签 (annotated)
* 创建一个轻量级的标签
    * **git tag v1.0.1**
* 创建一个带有附注的标签
    * **git tag -a v1.0.2 -m "release version"**
* 删除标签
    * **git tag -d tag_name**

* 查找标签
    * **git tag -l "v"**
* 查看所有的标签
    * **git tag**



### Diff (查看两个文件之间的差异性)

```bash
# 1. 暂存区与工作区文件之间的差别
git diff

diff --git a/test.txt b/test.txt
index dd2cc9d..5a9deda 100644
--- a/test.txt # 暂存区的文件
+++ b/test.txt # 工作区的文件
@@ -2,3 +2,4 @@ hello world
 hello Java
 Hello Python
 testing
+hello

# 2. 最新的提交(commit_id)与工作区文件的差别
git diff HEAD(commit_id)

diff --git a/test.txt b/test.txt
index 5a9deda..dd2cc9d 100644
--- a/test.txt
+++ b/test.txt
@@ -2,4 +2,3 @@ hello world
 hello Java
 Hello Python
 testing
-hello

# 3. 暂存区与版本库文件的差别
git diff --cached (commit_id)

git diff --cached HEAD
diff --git a/test.txt b/test.txt
index 5a9deda..dd2cc9d 100644
--- a/test.txt
+++ b/test.txt
@@ -2,4 +2,3 @@ hello world
 hello Java
 Hello Python
 testing
-hello
```



## 远程 & github

```bash
# 从远程仓库将代码拉取到本地, 同时会执行 merge
git pull == fetch + merge

# 将本地代码推送到远程仓库
git push

# 将远程仓库地址添加到本地
git remote add origin https://github.com/githubdemo-a/git_tutorial.git

# 将本地的 master 分支与远程仓库的 master 分支做一次关联，下次只要使用 git push 就行
git push -u origin master

# 列出所有远程仓库的别名
git remote show

# 显示远程仓库的详细信息
git remote show origin

# 生成一个 ssh private/public key
ssh-keygen
```



## 基于 Git 分支的开发模型

* develop 分支 (频繁变化) : 用于开发人员之间的代码合并等操作
* test 分支 (变化不是特别频繁) : 提供测试与产品等人员使用的分支
* master 分支 (生成发布分支, 变化非常不频繁)
* bugfix (hotfix) 分支 : 生产系统当中出现了紧急Bug, 用于紧急修复的分支

