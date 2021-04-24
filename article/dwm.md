---
title: dwm快捷键
date: 2020-10-11 22:11:33
categories:
- linux
---

## 基础快捷键
- 打开新终端
`Alt + shift + Enter`

- 关闭一个窗口
`Alt + shift + C`

- 窗口横向排列
`Alt + D`

- 窗口竖向排列
`Alt + I`

- 窗口位置互换
`Alt + Enter`

- 在窗口间切换
`Alt + J`
`Alt + k`

- 改变窗口的长度/比例
`Alt + H`
`Alt + L`

- 平铺模式（tiling)
`Alt + T`

- 单窗口模式
`Alt + M`

- 浮动模式（float)
`Alt + F`

- 窗口模式切换
`Alt + 空格`
`Alt + shift + 空格`

### 多屏幕问题
在主副屏之间移动焦点

```
# 移动焦点至左边屏幕
Mod + < 
# 移动焦点至右边屏幕
Mod + >
```

在主副屏之间移动窗口

```
# 移动窗口至左边屏幕
Mod + shift + <
```

- 切换标签页

`Mod + num`

- 移动窗口至某标签页

`Mod + shift + num`

### 自定义快捷键
我们以flamshot为例，为flameshot设置截图快捷键

首先在/* commands*/下添加你要设置快捷键的命令
- flameshot的截图命令是：flameshot gui，后面没有参数所以设为NULL，这条命令的名字我们设为flameshot

`static const char *flameshot[]  = { "flameshot","gui", NULL };`

进行快捷键和命令的绑定

```
# 我们设定截图的快捷键是：Alt + Ctrl + A(MODKEY对应Alt，ControlMask对应Ctrl，XK_a对应A)。然后将该快捷键绑定到名字为flameshot的命令。spawn自己百度
{ MODKEY|ControlMask,           XK_a,      spawn,          {.v = flameshot } },
```
