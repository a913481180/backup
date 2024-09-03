---
title: uni-App
date: 2020-01-22 23:33:33
categores:
  - web
tags:
  - web
---

# uniApp

## 安装

- 配置浏览器目录
- 微信小程序-》设置-》安全设置-》打开多端口服务

## 尺寸单位

- 支持的通用 css 单位包括 px、rpx

- px 即屏幕像素
  rpx 即响应式 px，一种根据屏幕宽度自适应的动态单位。以 750 宽的屏幕为基准，750rpx 恰好为屏幕宽度。屏幕变宽，rpx 实际显示效果会等比放大，但在 App（vue2 不含 nvue） 端和 H5（vue2） 端屏幕宽度达到 960px 时，默认将按照 375px 的屏幕宽度进行计算，具体配置参考

vue 页面支持下面这些普通 H5 单位，但在 nvue 里不支持：

rem 根字体大小可以通过 page-meta 配置
vh viewpoint height，视窗高度，1vh 等于视窗高度的 1%
vw viewpoint width，视窗宽度，1vw 等于视窗宽度的 1%
nvue 还不支持百分比单位。

App 端，在 pages.json 里的 titleNView 或页面里写的 plus api 中涉及的单位，只支持 px。注意此时不支持 rpx
nvue 中，uni-app 模式（nvue 不同编译模式介绍）可以使用 px 、rpx，表现与 vue 中基本一致，另外启用 dynamicRpx 后可以适配屏幕大小动态变化。weex 模式目前遵循 weex 的单位，它的单位比较特殊：

px:，以 750 宽的屏幕为基准动态计算的长度单位，与 vue 页面中的 rpx 理念相同。（一定要注意 weex 模式的 px，和 vue 里的 px 逻辑不一样。）
wx：与设备屏幕宽度无关的长度单位，与 vue 页面中的 px 理念相同
下面对 rpx 详细说明：

设计师在提供设计图时，一般只提供一个分辨率的图。

严格按设计图标注的 px 做开发，在不同宽度的手机上界面很容易变形。

而且主要是宽度变形。高度一般因为有滚动条，不容易出问题。由此，引发了较强的动态宽度单位需求。
rpx 是相对于基准宽度的单位，可以根据屏幕宽度进行自适应。uni-app 规定屏幕基准宽度 750rpx。

开发者可以通过设计稿基准宽度计算页面元素 rpx 值，设计稿 1px 与框架样式 1rpx 转换公式如下：

设计稿 1px / 设计稿基准宽度 = 框架样式 1rpx / 750rpx

换言之，页面元素宽度在 uni-app 中的宽度计算公式：

750 \* 元素在设计稿中的宽度 / 设计稿基准宽度

举例说明：

若设计稿宽度为 750px，元素 A 在设计稿上的宽度为 100px，那么元素 A 在 uni-app 里面的宽度应该设为：750 _ 100 / 750，结果为：100rpx。
若设计稿宽度为 640px，元素 A 在设计稿上的宽度为 100px，那么元素 A 在 uni-app 里面的宽度应该设为：750 _ 100 / 640，结果为：117rpx。
若设计稿宽度为 375px，元素 B 在设计稿上的宽度为 200px，那么元素 B 在 uni-app 里面的宽度应该设为：750 \* 200 / 375，结果为：400rpx。
Tips

注意 rpx 是和宽度相关的单位，屏幕越宽，该值实际像素越大。如不想根据屏幕宽度缩放，则应该使用 px 单位。
如果开发者在字体或高度中也使用了 rpx ，那么需注意这样的写法意味着随着屏幕变宽，字体会变大、高度会变大。如果你需要固定高度，则应该使用 px 。
rpx 不支持动态横竖屏切换计算，使用 rpx 建议锁定屏幕方向
设计师可以用 iPhone6 作为视觉稿的标准。
如果设计稿不是 750px，HBuilderX 提供了自动换算的工具，详见：HBuilderX 中自动转换 px 为 upx。
App 端，在 pages.json 里的 titleNView 或页面里写的 plus api 中涉及的单位，只支持 px，不支持 rpx。
早期 uni-app 提供了 upx ，目前已经推荐统一改为 rpx 了.

## 选择器

目前支持的选择器有：`.class` `#id` `element` (`::after` `::before` )仅 vue 页面生效
注意：

在 uni-app 中不能使用 \* 选择器。

微信小程序自定义组件中仅支持 class 选择器

page 相当于 body 节点，例如：

## 路由

uni-app 页面路由为框架统一管理，开发者需要在 pages.json 里配置每个路由页面的路径及页面样式。类似小程序在 app.json 中配置页面路由一样。所以 uni-app 的路由用法与 Vue Router 不同，如仍希望采用 Vue Router 方式管理路由，可在插件市场搜索 Vue-Router。

#路由跳转
uni-app 有两种页面路由跳转方式：使用 navigator 组件跳转、调用 API 跳转

|路由方式| 页面栈表现| 触发时机|
|初始化 |新页面入栈 uni-app 打开的第一个页面|
|打开新页面 |新页面入栈 |调用 API uni.navigateTo 、使用组件 `<navigator open-type="navigate"/>`|
|页面重定向 |当前页面出栈，新页面入栈 |调用 API uni.redirectTo 、使用组件 `<navigator open-type="redirectTo"/>`|
|页面返回 |页面不断出栈，直到目标返回页 |调用 API uni.navigateBack 、使用组件 `<navigator open-type="navigateBack"/>` 、用户按左上角返回按钮、安卓用户点击物理 back 按键|
|Tab 切换 |页面全部出栈，只留下新的 Tab 页面 |调用 API uni.switchTab 、使用组件 `<navigator open-type="switchTab"/>` 、用户切换 Tab|
|重加载 |页面全部出栈，只留下新的页面 |调用 API uni.reLaunch 、使用组件 `<navigator open-type="reLaunch"/>`|

## pages.json 文件

pages.json 文件用来对 uni-app 进行全局配置，决定页面文件的路径、窗口样式、原生的导航栏、底部的原生 tabbar 等。

它类似微信小程序中 app.json 的页面管理部分。注意定位权限申请等原属于 app.json 的内容，在 uni-app 中是在 manifest 中配置。

## 组件

-
