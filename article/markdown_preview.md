---
title: vim插件：markdown文件预览
date: 2020-02-22 11:22:11
categories:
- linux
---
## 安装Vundle插件管理器
github搜索Vundle，直接克隆
```
mkdir -p ~/.vim/bundle
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
## 编辑.vimrc配置文件
若没有则创建
在.vimrc中输入
```
set nocompatible   "去除vi一致性
filetype off 

"""Vundle插件位置
set rtp+=~/.vim/bundle/Vundle.vim
""初始化
call vundle#begin()

Plugin 'VundleVim/Vundle.vim'

"''''''''''''''''''''''''''
"其他插件安装位置"
"格式：Plugin '用户名/插件仓库名'


""""""""""''''''''""""""""""
call vundle#end()
filetype plugin indent on "加载vim自带的与插件的语法脚本等
```

## 安装插件

```
:pluginInstall  //安装插件
:PluginClean  	//删除插件
:PluginUpdate   //更新
:PluginSearch   //搜索
```
