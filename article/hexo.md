---
title: hexo搭建博客
date: 2020-10-11 22:11:22
categories:
- blog
---

## 安装nodejs
* archlinux:`pacman -S nodejs`
* ubuntu 

## 安装npm
* archlinux:
`pacman -S npm`
* ubuntu: 
`apt-get install npm `

### 换源

`npm config set registry https://registry.npm.taobao.org  //添加淘宝源`

使用nrm切换镜像源

`npm install nrm -g //全局安装`

`nrm ls  //查看所有镜像源`

`nrm use taobao  //切换为淘宝镜像源`

查看镜像源：`npm get registry`

安装hexo

`npm install -g hexo-cli`

## ssh
查看ssh

`cd ~/.ssh`

若无，则生成ssh

`ssh-keygen -t rsa -C 用户名`

一路回车默认安装,打开生成的`id_rsa.pub`复制

设置git用户名邮箱  

```
git config --global user.name "你的账号"
git config --global user.email "你邮箱地址"
```

## hexo
进入一个文件夹进行初始化：`hexo init`

编辑`_config.yml`
找到

```
deploy:
  type:
```

修改为：

```
deploy:
  type: git
  repository: ........... ///克隆地址
  branchL: master
```

执行`hexo g -d `上传代码（注意第一次提交需要输入yes，不要一路回车）

**常用命令：**

```
hexo generate	//Generate static files
hexo server	//Run server
hexo deploy	//Deploy to remote sites

hexo new My_New_Post   	//Create a new post
```
### hexo博客迁移

先新建一个文件夹，初始化后，从原来的文件夹中拷贝下列文件进行覆盖
```
themes/
_config.yml
------------------
package.json
scaffolds/
source/
```

新建分类、搜索、关于界面：
```
hexo new page categories
//进入source/categories/文件夹
//编辑index.md文件,添加type或layout属性
---
title: categories
layout: categories
或
type: categories
---
```

### 常见问题

```
$ hexo d
ERROR Deployer not found: git
```
解决办法：
`npm install --save hexo-deployer-git`
