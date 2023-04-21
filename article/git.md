---
title: git
date: 2020-12-12 12:12:12
categories:
  - blog
---

## 安装 git

`sudo apt-get install git`

## 配置 git

```
git config --global user.name "用户名"               //个人账号
git config --global user.email 123456@qq.com         //个人邮箱地址
上面的--global选项,表示以后管理git库时,默认使用上面的用户信息,也可以通过git config -l 来查看配置信息
```

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

```
# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy

#刷新dns
ipconfig /flushdns

```
