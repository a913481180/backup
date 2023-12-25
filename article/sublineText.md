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

支持Less，执行以下命令安装：`yarn add -D less`
引入Ant Design 5.x：`yarn add antd`
设置Antd为中文语言：

```jsx
    import ReactDOM from 'react-dom/client'
    import App from './App'
   import { ConfigProvider } from 'antd'
   // 引入Ant Design中文语言包
  import zhCN from 'antd/locale/zh_CN'
    // 全局样式
    import '@/common/styles/frame.styl'
    
    const root = ReactDOM.createRoot(document.getElementById('root'))
   root.render(
       <ConfigProvider locale={zhCN}>
          <App />
       </ConfigProvider>
   )
```
