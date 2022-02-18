---
title: subline text 编辑器
date: 2021-11-12 20:21:33
categories: 
- tool
---
# subline text 编辑器使用

## 安装package control

1. 首先打开编辑器，然后找到顶部菜单的`Tools`菜单
2. 然后选择`Tools`菜单下面的`Command Paletter`选项
3. 在打开的命令行模式输入框中输入`pac`,然后回车安装
4. 稍等片刻以后,软件会提示信息,安装完了重新启动软件
5. 重启以后,在菜单栏的`preferences`菜单下你会看到`Package Settings`和`Package Control`两个选项，以后会用到他们

## 设置为中文

1. 点击Package Control选项
2. 选择installPackage
3. 搜索ChineseLocalizations
4. 点击安装

## 代码高亮

1. 将鼠标移至右下角的按钮区、点击
2. 在弹出的菜单中找到当前文件类型
3. 如果你想每次打开页面都出现语法高亮，可以将鼠标移至顶部“open all with current extension as...”，在弹出选项中选择“html”（按照你常用的文件类型设置），即可再下次打开文件后就出现高亮显示

## 设置自动保存

1. 点击`Preferences`里的设置-用户
2. `ctrl+f`找到`save_on_focus_lost`，把后面的`false`改成`true`就好

## 设置vim模式

1. 点击`Preferences`里的设置-用户
2. 编辑`Preferences.sublime-settings--User`文件
3. 设置中的ignored_packages对应的列表中去掉Vintage这一项,没有ignored_packages这项就添加

```
"ignored_packages":
[
//"Vintage"
],
```
## 分屏

1. `Shift+Alt`+`1,2,3,4,5`

2. 文件克隆,拖放到另一个窗口上

##关闭更新

`Preferencens`设置用户, 添加`"update_check": false`

## 关闭缩略图

关闭这个缩略图,需要到菜单视图里面设置，首先点击菜单栏`view`——`hideMiniMap`

## 格式化

按键绑定-用户->打开后在最后面添加

`{ "keys": ["ctrl+shift+q"], "command": "reindent"},`

## sublime 中用正则 去除空行、html注释和js注释

1. 去除空行

`CTRL+H`打开`replace`功能，勾选上左侧的`regular expression`，并填写 

`find what`栏 : `\s+$`  （正则表达式）
`replace with`栏 : （这行留空） 

接着点`replace all`即可


2. 去除html注释

`CTRL+H`打开`replace`功能，勾选上左侧的`regular expression`，并填写 

`find what`栏 :` <!--[\s\S]*?-->`  （正则表达式）
`replace with`栏 : （这行留空） 

接着点`replace all`即可


3. 去除js注释

`CTRL+H`打开`replace`功能，勾选上左侧的`regular expression`，并填写 

`find what`栏 : `/\*[\s\S]*?/ ` （正则表达式）
`replace with`栏 : （这行留空） 

接着点`replace all`即可


## 解决文件名方格乱码

可以在Sublime的user-settings中，覆盖默认的dpi，让Sublime以一个较小的文字显示

1. 点击Preferences –> Settings–User

2. 在最后一行加上"dpi_scale": 1.0 （注意：在加上最后一段的时候，前面的字段要加上逗号“，”这是Sublime自己的命名规范）

 