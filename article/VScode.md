---
title: VScode
date: 2021-08-22 20:22:11
categories:
  - tool
---

# vscode

## 设置

### VScode 关闭右侧预览功能

在设置中搜索"editor.minimap.enabled"把勾去掉

### 修改 vscode 配置文件改字体大小

打开 VSCode 安装目录下的如下文件`VSCode\resources\app\out\vs\workbench\workbench.desktop.main.css` 查找 `.part>.content` 修改字体大小即可

### vim 插件设置

```json
{
  //vim 前导键
  "vim.leader": "<space>",
  //启用系统剪切板作为vim寄存器
  "vim.useSystemClipboard": true,
  //将按钮交给vscode处理
  "vim.handleKeys": {
    "<C-f>": false
  }
}
```

### 自动保存

打开 vscode 窗口，单击左下角的设置(齿轮状)图标
在设定按钮显示的菜单中，选择【Settings】选项。 这里是整个 vs 代码的设置入口
打开【settings】画面时，默认的【Auto Save】=【off】表示不自动保存，每次都需要用户自己手动保存

#### 【后延迟】

1. 这是以一定的间隔自动保存的
2. 该【自动保存】选项需要指定间隔时间，以匹配下一个【自动保存延迟】配置项目，单位为毫秒
   【onFocusChange】
3. 当焦点偏离编辑器当前窗口时自动保存
4. 即，即使在编辑器内部切换标签，也会触发自动保存
5. 本项目不需要【自动保存延迟】的设定值，此值也会被忽略

#### 【onWindowChange】

1. 编辑器窗口失去焦点时，自动保存
2. 只有在焦点偏离整个编辑器时才会触发保存，在编辑器内部切换标签时不会自动保存
3. 本项目不需要【自动保存延迟】的设定值，此值也会被忽略
   在【Settings】画面中修正值后，不点击追加保存按钮，自动保存设定的内容。

## vscode 好用的插件系列

### 汉化插件

- `Chinese (Simplified)`

### VIM

- `Vim`

### git 相关插件

- `gitLens`
- `Git History`
- `Git History Diff`
- `Git Blanme`

### 格式化类插件

- `Prettier - Code formatter`
- `Vetur`、

### 规范检查

- `ESLint`
- `Stylelint`
- `TSLint（已废弃）`

### 标签自动化三剑客

- `Auto Rename Tag`
- `Auto Import`
- `Auto Close Tag`：最新 vscode 已支持

### css 模块跳转

- `CSS Modules`

### 代码片段提示

- `ES7+ React/Redux/React-Native snippets`
- `HTML CSS Support`
- `JavaScript (ES6) code snippets`
- `node-snippets`

### Markdown 插件

- `Markdown Preview Enhanced`
- `Markdown All in One`
- `markdownlint`

### 单词拼写

- `Code Spell Checker`

### 浏览器打开

- `open in browser`
- `live Server`

### 主题

- `GitHub Theme`

### 图片预览

- `Image Preview`

### 快速生成注释

- `koroFileHeader`

### 路径提示

- `Path Intellisense`：最新 vscode 已支持

### npm 模块导入智能提示

- `npm Intellisense`

### 接口请求

- `REST Client`

### SSH

- `Remote-SSH`
- `Visual Studio Code Remote - SSH: Editing Configuration Files`
- `Remote Explorer`

### 错误显示

- `Error Lens`
