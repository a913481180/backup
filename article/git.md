---
title: git
date: 2020-12-12 12:12:12
categories:
- linux
- git
---

## 安装git
`sudo apt-get install git`
## 配置git
```
git config --global user.name "用户名"               //个人账号
git config --global user.email 123456@qq.com         //个人邮箱地址
上面的--global选项,表示以后管理git库时,默认使用上面的用户信息,也可以通过git config -l 来查看配置信息
```
## git命令

对当前目录进行git管理,该目录便会成为工作区,并在当前目录下会出现个.git隐藏目录.
该.git里会保存git所需要的数据和资源,也就是git仓库和暂存区都会保存在.git里
`git init`
由于git clone会自动生成.git隐藏目录,所以上面无需git init命令初始化。
而且.git目录里保存了远程仓库地址,所以上面无需git remote命令连接远端。


克隆复制远程仓库到当前目录
`git clone https://......`

查看工作目录下文件的状态,是否add添加到了暂存区
`git status`

将工作区修改的文件添加到暂存区也可以使用git add .  将所有修改的文件进行添加
`git add file`

将暂存区的文件删除掉,若工作区文件存在,则需要使用git rm -f file来强制删除掉
`git rm file`

将暂存区里的文件提交到本地仓库,若暂存区某个文件被删除掉,则会删除仓库里的文件。加参数 -a:跳过暂存区,git自动将工作区里记录的所有文件添加到暂存区并一起提交,从而跳过git add步骤
`git commit -m "description"`

撤销commit,如果想修改commit时的文件
`git reset HEAD^1`

添加远程仓库(origin),也就是关联本地仓库和github仓库
`git remote add origin https://.......`

推送(同步)数据到远程仓库,若是本地上传,必须先使用上个命令来指定远程仓库地址(origin),
若是从远程仓库克隆复制的,则不需要,因为.git里会自动保存远程仓库地址.
`git push origin 分支`

## ssh key

首先通过ls ~/.ssh命令,查看是否已有ssh key,若有的话,先备份,然后通过rm -rf ~/.ssh来删除
1. 输入,创建密钥
`ssh-keygen -t rsa -C "...."    // "..."里输入邮箱号`
2. 然后会提示设置密码,直接连按3个回车,表示密码为空

3. 将新生成的key添加到ssh-agent中

`eval  "ssh-agent -s"`
`ssh-add ~/.ssh/id_rsa`
4. 若显示Could not open a connection to your authentication agent.,则继续输入
`ssh-agent bash`
`ssh-add ~/.ssh/id_rsa`
出现Identity added字段,则表示写入成功,ssh key公钥便保存在id_rsa.pub文件中了
5. 将复制的ssh key公钥添加到github中
6. 测试ssh key
`ssh git@github.com`
显示Hi  youname! ,则表示成功

