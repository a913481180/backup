---
title: VScode
date: 2021-08-22 20:22:11
categories:
- tool 
---

- VScode关闭右侧预览功能：在设置中搜索"editor.minimap.enabled"把勾去掉
- 修改vscode配置文件改字体大小：打开VSCode安装目录下的如下文件VSCode\resources\app\out\vs\workbench\workbench.desktop.main.css查找 .part>.content修改字体大小即可
- 自动保存：打开vscode窗口，单击左下角的设置(齿轮状)图标

在设定按钮显示的菜单中，选择【Settings】选项。 这里是整个vs代码的设置入口

打开【settings】画面时，默认的【Auto Save】=【off】表示不自动保存，每次都需要用户自己手动保存

【后延迟】

1 )这是以一定的间隔自动保存的

2 )该【自动保存】选项需要指定间隔时间，以匹配下一个【自动保存延迟】配置项目，单位为毫秒

【onFocusChange】

1 )当焦点偏离编辑器当前窗口时自动保存

2 )即，即使在编辑器内部切换标签，也会触发自动保存

3 )本项目不需要【自动保存延迟】的设定值，此值也会被忽略

【onWindowChange】

1 )编辑器窗口失去焦点时，自动保存

2 )只有在焦点偏离整个编辑器时才会触发保存，在编辑器内部切换标签时不会自动保存

3 )本项目不需要【自动保存延迟】的设定值，此值也会被忽略

在【Settings】画面中修正值后，不点击追加保存按钮，自动保存设定的内容。

## vscode好用的插件系列


git相关插件：gitLens、Git History、Git History Diff
汉化插件： Chinese (Simplified)
开发提示相关插件： Tabnine AI（强推）、Path Intellisense（路径提示）、Vetur、Volar、Auto Close Tag、Auto Complete Tag、Auto Rename Tag、HTML Snippets、Wrap Console Log Lite
浏览器调试插件： JavaScript Debugger、Debug Visualizer、scode-js-debug
静态服务器：Live Server
代码运行器： Code Runner
正则提示插件： any-rule、Regex Previewer（辅助验证正则结果）
Docker插件： Docker
格式化类插件： EditorConfig for VS Code、ESLint
进制文件查看： Hex Editor
TODO提示： Todo Tree
流程图绘制： drawio
PDF查看： vscode-pdf
Markdown预览：Markdown Preview Enhanced
SVG文件预览：SVG Viewer
图片预览（在html或者css写地址的时候可直接预览）：Image Preview
文件依赖分析（如：.vue文件可以通过该插件看到引用了哪些依赖）：Dependency Cruiser Extension
颜色选择、设置：Color Highlight、Color Picker
快速生成注释：vscode-fileheader 和 koroFileHeader 
npm模块导入智能提示： npm Intellisense
接口请求：REST Client
