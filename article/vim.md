---
title: Vim常用操作
date: 2020-12-01 20:00:00
categories:
  - linux
tags:
  - linux
---

## 常用功能键

1. 插入模式

i ：在光标前插内内容:按 8，再按 i，进入插入模式，输入=， 按 esc 进入命令模式，就会出现 8 个=。

a ：在光标后插入内容
A : 在当前行最后插入；

o ：在所在行的下一行插入新行
O ：在所在行的上一行插入新行

2. 删除文本

x：删除光标后面的字符
X：删除光标前面的字符

nx：删除光标后面 n 个字符
nX：删除光标前面的 n 个字符

d0：删除光标至行首的内容
d$：删除光标至行尾的内容

dd ：删除整行
ndd ：向下删除 n 行

3. 移动光标

vi 可以直接用键盘上的光标来上下左右移动，但正规的 vi 是用小写英文字母「h」、「j」、「k」、「l」，分别控制光标左、下、上、右移一格。

按「ctrl」+「b」：屏幕往“后”移动一页。

按「ctrl」+「f」：屏幕往“前”移动一页。

按「ctrl」+「u」：屏幕往“后”移动半页。

按「ctrl」+「d」：屏幕往“前”移动半页。

按数字「gg」:移动到文章开头
按「G」：移动到文章的最后。

按「$」：移动到光标所在行的“行尾”。

按「0」：移动到光标所在行的“行首”

按「w」：光标跳到下个字的开头

按「e」：光标跳到下个字的字尾

按「b」：光标回到上个字的开头

按「nl」：光标移到该行的第 n 个位置，如：5l,56l。

4. 查找文本

/pattern ：向下查找

?pattern ：向上查找

n ：顺序查找

N ：反向查找

:s/p1/p2/g ：在当前行，将 p1 替换成 p2

:n1,n2s/p1/p2/g ：将 n1 至 n2 行之间的 p1 替换成 p2 5. 复制
「yw」：将光标所在之处到字尾的字符复制到缓冲区中。
「nyw」：复制 n 个字到缓冲区
yy ：复制整行

nyy ：复制 n 行

p ：在所在行下一行粘贴

P ：在所在行上一行粘贴

在 Vim 中你可以把两行合并为一行，也就是说两行之间的换行符被删除了：命令是"J"。
dd ：剪切

复制 vim 里面的内容至系统粘贴板："+y

粘贴系统粘贴板里面的内容至 vim："+p

:n1,n2 co n3 ：将 n1 至 n2 行复制到 n3 行的下面

:n1,n2 m n3 ：将 n1 至 n2 行剪切至 n3 行的下面

:n1,n2 d ：将 n1 至 n2 行删除 6. 替换
「r」：替换光标所在处的字符。
「R」：替换光标所到之处的字符，直到按下「ESC」键为止。

7. 回复上一次操作
   「u」：如果您误执行一个命令，可以马上按下「u」，回到上一个操作。按多次“u”可以执行多次回复。
   CTRL-R(重做)来反转撤消的动作

8. 更改
   「cw」：更改光标所在处的字到字尾处
   「c#w」：例如，「c3w」表示更改 3 个字

9. 跳至指定的行
   「ctrl」+「g」列出光标所在行的行号。
   「#G」：例如，「15G」，表示移动光标至文章的第 15 行行首。

跳到文件中的某一行
「#」：「#」号表示一个数字，在冒号后输入一个数字，再按回车键就会跳到该行了 10. 宏
. --重复上一个编辑动作
qa：开始录制宏 a（键盘操作记录）
q：停止录制
@a：播放宏 a

11. vi 命令列表
    ctrl+v 进入可视块模式，之后使用 j/k/h/l 键可以选中一块
    ggVG 选中全部的文本， 其中 gg 为跳到行首，V 选中整行，G 末尾
    :pwd 显示 vim 的工作目录
    :Sex -- 水平分割一个窗口，浏览文件系统；
    :Vex -- 垂直分割一个窗口，浏览文件系统
    :ce(nter) 本行文字居中
    :le(ft) 本行文字靠左
    :ri(ght) 本行文字靠右

## 命令

设置 backspace 的工作方式
:set backspace=indent,eol,start
:set spell－开启拼写检查功能
:set nospell－关闭拼写检查功能
「set nu」后，会在文件中的每一行前面列出行号。
高亮显示搜索结果用":set hlsearch"
:set ruler 会在屏幕右下角显示当前光标所处位置

## Gvim 配置

```txt
（“为注释）
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 代码补全
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
set wildmenu        "vim自身命令行模式智能补全
set completeopt-=preview    "补全时不显示窗口，只显示补全列表

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"代码折叠
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
set foldmethod=syntax       "设置基于语法进行代码折叠
set nofoldenable            "关闭代码折叠
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 缓存设置
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
set nobackup        "设置不备份
set noswapfile      "禁止生成临时文件
set noundofile  " 操作记录
set autoread        "文件在vim之外修改过，自动重新载入
set autowrite       "设置自动保存
set confirm         "在处理未保存或只读文件时，弹出确认

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 编码设置
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" Lang & Encoding {{{
set fileencodings=utf-8,gbk2312,gbk,gb18030,cp936
set encoding=utf-8
set langmenu=zh_CN
let $LANG = 'en_US.UTF-8'
"language messages zh_CN.UTF-8
" }}}
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 隐藏GVIM菜单及设置
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
source $VIMRUNTIME/delmenu.vim
source $VIMRUNTIME/menu.vim
" 窗口大小
set lines=35 columns=140
" 分割出来的窗口位于当前窗口下边/右边
set splitbelow
set splitright
"不显示工具/菜单栏
set guioptions-=T
set guioptions-=m
set guioptions-=L
set guioptions-=r
set guioptions-=b
" 使用内置 tab 样式而不是 gui
set guioptions-=e
set nolist

set nu
set guifont=Consolas:h11:cANSI:qDRAFT
```
