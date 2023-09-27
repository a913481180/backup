---
title: git
date: 2020-12-12 12:12:12
categories:
  - blog
---

## 安装 git

`sudo apt-get install git`

## 配置 git

```bash
git config --global user.name "用户名"               //个人账号
git config --global user.email 123456@qq.com         //个人邮箱地址
# 上面的--global选项,表示以后管理git库时,默认使用上面的用户信息,也可以通过git config -l 来查看配置信息
```

## 分支操作

创建分支

```bash
创建分支: git branch 分支名
切换分支: git checkout 分支名
查看所有分支: git brach -a
分支合并: git merge 分支名 (把分支合并到当前分支)
```

删除分支

```bash
首先使用git branch -a查看当前所有分支。
删除本地分支：git branch -d 分支1[,分支名2,分支名3...]
删除远程分支: git push origin --delete 分支名1[,分支命2,分支名...]
```

## 将 master 分支合并到当前迭代分支

1、切换到master分支，拉取最新代码到本地

```bash
#切换到master分支
git checkout master
#拉取最新代码
git pull
```

2、切换到当前开发分支，合并master分支

```bash
#切换到当前分支
git checkout myBranch
#合并
git merge master
```

3、代码冲突解决
这一步需要在编辑器上，把冲突部分解决掉（这一步特别重要，最好叫上相关负责人一起，防止弄丢master里的最新代码）

4、当前开发分支代码提交

```bash
git add .
git status
git commit -m"merge from master"
git push origin
```

## 合并其他分支代码至master分支

1.当前分支所有代码提交
先将dev分支上所有有代码提交至git上，提交的命令一般就是这几个，先复习下：

```bash
# 将所有代码提交
git add .

# 编写提交备注
git commit -m "修改bug"

# 提交代码至远程分支
git push origin dev
```

2.切换当前分支至主干（master）

```bash
# 切换分支
git checkout master

# 如果多人开发建议执行如下命令,拉取最新的代码
git pull origin master
```

3.合并（merge)分支代码

```bash
git merge dev
# merge完成后可执行如下命令，查看是否有冲突
git status
```

4.提交代码至主干（master）
`git push origin master`

5.最后切换回原开发分支
`git checkout dev`

## 放弃本地更改

1. 没有添加到暂存区

没有执行git add , 可以用命令

```bash
git checkout – filepathname（eg: git checkout – test.cpp）
```

如果是放弃所有，直接执行

```bash
git checkout .    
```

此命令用来放弃掉所有还没有加入到缓存区（就是 git add 命令）的修改：新增的文件会被删除、删除的文件会恢复回来、修改的文件会回去。回到暂存区之前的样子。对之前保存在暂存区里的代码不会有任何影响。对commit提交到本地分支的代码就更没影响了。当然，如果你之前压根都没有暂存或commit，那就是回到你上次pull下来的样子了。

但是此命令不会删除掉刚新建的文件。因为刚新建的文件还没已有加入到 git 的管理系统中。所以对于git是未知的。自己手动删除就好了。

2. 已经添加到暂存区：
已经使用git add添加到暂存区，可以用命令

```bash
git reset HEAD filepathname （比如： git reset HEAD readme.md）
```

同样放弃所有就是

```bash
git reset HEAD .
```

执行完此命令后，文件状态就回归到第一种情况了，此时再按照情况1处理。

3. 已经提交到本地仓库：
使用git commit提交到本地仓库，可以用命令回退到上一次commit的状态

```bash
git reset --hard HEAD^
```

可以用命令回退到任意版本：

```bash
git log # 得到你需要回退一次提交的commit id
git reset --hard commitid# 回到其中你想要的某个版
```

4. 放弃本地修改，强制和远程同步
在使用Git的过程中，有些时候我们只想要git服务器中的最新版本的项目，对于本地的项目中修改不做任何理会，就需要用到Git pull的强制覆盖，具体代码如下：

```bash
git fetch --all
git reset --hard origin/master 
git pull
```

## 回滚

1. 通过git reset是直接删除指定的commit

```bash
git log # 得到你需要回退一次提交的commit id

git reset --hard

git push origin HEAD --force # 强制提交一次，之前错误的提交就从远程仓库删除
```

2. 通过git revert是用一次新的commit来回滚之前的commit

```bash
git log # 得到你需要回退一次提交的commit id

git revert # 撤销指定的版本，撤销也会作为一次提交进行保存
```

3. git revert 和 git reset的区别

   - git revert是用一次新的commit来回滚之前的commit，此次提交之前的commit都会被保留；

   - git reset是回到某次提交，提交及之前的commit都会被保留，但是此commit id之后的修改都会被删除

## git 命令

对当前目录进行 git 管理,该目录便会成为工作区,并在当前目录下会出现个.git 隐藏目录.
该.git 里会保存 git 所需要的数据和资源,也就是 git 仓库和暂存区都会保存在.git 里

- `git init` 本地初始化，建立缓存区

由于 git clone 会自动生成.git 隐藏目录,所以上面无需 git init 命令初始化。
而且.git 目录里保存了远程仓库地址,所以上面无需 git remote 命令连接远端。

- `git add filename`
  将暂存区里的文件提交到本地仓库,若暂存区某个文件被删除掉,则会删除仓库里的文件。加参数 -a:跳过暂存区,git 自动将工作区里记录的所有文件添加到暂存区并一起提交,从而跳过 git add 步骤
  将工作区修改的文件添加到暂存区也可以使用`git add .` 将所有修改的文件进行添加;或`git add *`提交所有文件。

- `git rm filename`
  将暂存区的文件删除掉,若工作区文件存在,则需要使用`git rm -f file`来强制删除掉,

- `git commit -m "your description"`
  为本次操作添加描述

- `git clone https://......`
  克隆复制远程仓库到当前目录

- `git status`
  查看工作目录下文件的状态

- `git checkout filename`
  从暂存区恢复文件到工作区

- `git diff`
  查看工作区和暂存区版本区别

- `git log`
  查看已提交到暂存区的历史版本

- `git reset --hard 版本号`
  恢复文件到指定的版本

-`git remote add origin https://.......`
添加远程仓库(origin),也就是关联本地仓库和 github 仓库

- `git push origin 分支`
  推送(同步)数据到远程仓库,若是本地上传,必须先使用上个命令来指定远程仓库地址(origin),
  若是从远程仓库克隆复制的,则不需要,因为.git 里会自动保存远程仓库地址.

- `git pull`
  从远程仓库中同步代码

## 常见问题

- git 提交=报错 443

```bash
# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy

#刷新dns
ipconfig /flushdns

```
