# Git使用

## 配置user.name和 user.email
```
git config --global user.name "your_name"
git config --global user.email "your_email@gmail.com"
```

`git config --local`  local只对某个仓库有效

`git config --global`  global对当前用户所有仓库有效

`git config --system`  system对系统所用登陆的用户有效

## 显示config的配置，加 --list
```
git config --list --local
git config --list --global
git config --list --system
```

## 创建第一个仓库

* 把已有的项目代码纳入管理
  ```
  cd 项目代码所在的文件夹
  git init
  ```
* 新建的项目直接用 git 管理
  ```
  cd 某个文件夹
  git init your_project  会在当前路径下创建和项目名称同名的文件夹
  cd your_project
  ```


## 通过几次 commit 来认识工作区和暂存区

工作目录 --> git add files --> 暂存区 --> git commit --> 版本历史

```
git status    查看当前状态
git add       把修改添加到暂存区
git commit    添加修改说明
git log       查看提交历史
```

## 给文件重命名的方式
`mv readme readme.md`  在 git 中表现为 先把 readme文件删除，然后新建的readme.md文件

`git mv readme readme.md`  使用 git mv 进行文件重命名

## 通过 git log 查看版本演变历史

```
git log
git log --oneline 查看简洁的变更历史
git log -n4       查看最近4次的变更
git log -n4 --oneline
```
其他常用命令：
```
git branch -v                        查看本地有多少分支
git checkout -b dev                  创建一个新的分支
git checkout -b temp dev             创建一个临时分支
git commit -am "test"                直接把工作区的变更提交到版本库
git log --oneline --all -n4 --graph  图形化的 git log
gitk                                 启动git的图形化截面
```

## 探秘 .git 目录
HEAD 文件记录了我们正在工作的分支

git checkout dev 切换到dev分支

config 文件 记录了 我们的配置信息
refs  记录设置的标签 tags，记录了我们创建的分支

objects目录

## git需要处理的一些情况
* 理解 HEAD 和 branch
  ```
  git diff
  ```
* 怎么删除不需要的分支
  ```
  git branch -d dev  删除一个分支
  git branch -D dev
  ```
* 怎么修改最新 commit 的 message
  ```
  git commit --amend "***"
  ```
* 怎么修改老旧文件的 commit 的 message
* 怎样把连续的多个 commit 整理成一个
* 怎样把间隔的几个 commit 合并成一个
* 怎么比较暂存区和 HEAD 所含文件的差异
  ```
  git diff --cached   一定要加 --cached
  ```
* 怎样比较工作区和暂存区所含文件的差异
  ```
  git diff
  git diff -- file.1 file.2
  ```
* 如何让暂存区恢复成和 HEAD 的一样
  ```
  git reset HEAD 取消暂存区的所有变更
  ```
* 如何让工作区的文件恢复为和暂存区一样
  ```
  git checkout -- index.php
  ```
* 怎样取消暂存区部分文件的更改
  ```
  git reset HEAD -- index.php
  ```
* 消除最近的几次提交
* 看看不同提交分支的指定文件的差异
  ```
  git diff dev master -- index.php  比较一个文件在不同分支中的差异
  ```
* 正确删除文件的方法
  ```
  git rm index.php
  ```
* 开发中临时加塞了紧急任务怎么处理
  ```
  git stash        把文件存入一个区域
  git statch list

  git stash apply   把文件变更取出来
  git stash pop
  git stash list   查看 stash 的内容
  ```
* 如何指定不需要 git 管理的文件
  ```
  .gitignore 文件中包含的文件不会被 git 管理
  ```

## 如何将 git 仓库备份到本地
```
git clone --bare /Users/git_learning/.git lss.git   克隆一个不带工作区的仓库并命名为lss.git
git clone --bare file:///Users/git_learning/.git lss.git  

git remote -v  查看远程仓库信息
git remote add git_one file:///Users/git_learning/git_one.git  将本地仓库 git_one 添加到远端

git push git_one  将仓库 push 到远端
```

## 本机与github配置公钥私钥
** 检查本地是否配置 ssh **

`cd ~/.ssh`  (win 系统)

** 生成一个 SSH Key **

`ssh-keygen -t rsa -b 4096 "your_email@example.com"`

## 把本地仓库同步到 Github
```
git remote -v    查看远程仓库信息
git remote add git github git@github.com:jiexialss/thinking.git
git push github --all    把本地仓库github 推到远端仓库

git fetch github master  把代码从远端的master分支拉回来，但不进行关联合并
git merge dev    合并分支到当前分支
git pull         把代码拉下来并进行何把

git checkout dev 切换分支到dev
```

## 需要重点关注的问题

### 不同人修改了不同文件如何处理

### 不同人修改了同文件的不同区域如何处理

### 不同人修改了同文件的同一区域如何处理
`会有冲突产生`

### 同时变更了文件名和文件内容如何处理
`git pull`

### 把同一文件改成不同的文件名如何处理
`会有冲突产生`

### 禁止向集成分支之下 push -f 操作
`push -f 是强制推到集成分支上`

### 禁止向集成分支执行变更历史的操作

### GitHub 都有哪些核心功能

### 如何高效的搜索到感兴趣的开源项目

### 怎样在github 上搭建个人博客

### 开源项目怎么保证代码质量

### GitHub 为什么需要组织类型的仓库

### GitHub创建团队的项目

### 怎样选择适合自己团队的工作流
`Git Flow` `GitHub Flow` `GitLab Flow`

### 如何选择合适的分支集成策略

### 启用 issue 跟踪需求和任务

### 项目内部怎么实施 code review 

### 团队协作如何做多分支的集成

## GitLab