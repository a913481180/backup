---
title: Electron
date: 2023-11-12 20:21:33
categories:
  - tool
---

## 安装

- `yarn create @quick-start/electron`

```bash
? Add TypeScript? » No / Yes
是否使用TypeScript？选No，本教程使用JavaScript。如果喜欢TypeScript，请选择Yes。
? Add Electron updater plugin? » No / Yes
是否添加Electron updater插件？选择Yes。
? Enable Electron download mirror proxy? » No / Yes
是否开启Electron镜像下载代理。在国内网络环境，强烈建议选择Yes。
```

支持 Less，执行以下命令安装：`yarn add -D less`
引入 Ant Design 5.x：`yarn add antd`
设置 Antd 为中文语言：

```jsx
import ReactDOM from "react-dom/client";
import App from "./App";
import { ConfigProvider } from "antd";
// 引入Ant Design中文语言包
import zhCN from "antd/locale/zh_CN";
// 全局样式
import "@/common/styles/frame.styl";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <ConfigProvider locale={zhCN}>
    <App />
  </ConfigProvider>
);
```

## 使用

## 问题

### 一、打开项目空白，重定向到 chrome-error://chromewebdat

问题原因：打包好之后打开.exe 文件，项目空白，控制台查看是重定向到 chrome-error://chromewebdat 这个路径下。控制台也不会进行报错！

说明：

electron 项目在开发阶段，因为使用 webpack / vite 脚手架,启用了 webServer 提供的 http 服务，所以有路由功能，当我们运行 npm run electron:serve 的时候，最后可以直接加载 http://localhost:8080。在终端里面就可以显示页面。

当 electron 项目打包之后，成为桌面程序，这个时候就没有 http 服务支撑，所以加载的是静态页面，win.loadURL("file://./index.html")。

解决方案：

1. 改变路由设置的模式，将 history 改为 hash，具体代码看下方写的《路由不跳转问题》

2. 根据判断生产环境 mainWindow.loadFile(path.resolve(\_\_dirname, '../dist/index.html')) 在 electron main.js 中 无法使用 （.env.production / .env.development）中配置的环境变量 所以安装一个 cross-env 插件配置。

### 三、打包之后，请求不到后端数据，有的人是 app:// 有的人是 file://

因为在本地环境 需要反向代理解决跨域问题，“/api”则是在 vite.config.js 中配置的 server。而生产环境配置的代理就不在管用了，需要将请求的路径(http://www.baidu.com)配置成baseURL，如果跨域的话 就要看后端是否已经做了跨域处理，下方 import.meta.env.MODE 三元判断区也可以用《上方所提到的 isDev》

在 axios 配置的文件添加：

const BASEURL = import.meta.env.MODE == 'development' ? '/api' : 'http://www.baidu.com';
axios.defaults.baseURL = BASEURL;
