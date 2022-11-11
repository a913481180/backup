---
title: 全栈项目
date: 2022-01-12 20:11:33
categories:
  - web
---

### 初始化

#### 创建前端项目

##### vue

`vue create 项目名称`

##### react

- 安装脚手架
  `npm i create-react-app -g`
- 创建文件夹并初始化
  `create-react-app test_project`
  `cd test_project`
  `npm run start`
- 安装路由
  `npm i react-router-dom`
  创建 router/index.js 文件,写入路由表

  ```
  import React from "react";
  import { lazy, Suspense,  } from "react";
  import { Navigate } from "react-router-dom";
  import LoadingPage from "../pages/LoadingPage";
  const Home = lazy(() => import("../pages/Home"));
  const ErrorBlock = lazy(() => import("../pages/ErrorBlock"));
  let route = [
  	{
  		path: "/Home",
  		element: (
  			<Suspense fallback={<LoadingPage />}>
  				<Home />
  			</Suspense>
  		),
  	},
  	{
  		path: "/ErrorBlock",
  		element: (
  			<Suspense fallback={<LoadingPage />}>
  				<ErrorBlock />
  			</Suspense>
  		),
  	},
  	{
  		path: "/",
  		element: <Navigate to="/Home"></Navigate>,
  	},
  ];
  export default route;
  ```

  入口文件 index.js 中加入

```
import {BrowserRouter} from 'react-router-dom'
<BrowserRouter>
<App/>
</BrowserRouter>
```

    App.js 中引入路由表

```
import './App.scss';
import route from './route/'
import { useRoutes } from 'react-router-dom'
function App() {
const element = useRoutes(route)
return (
<div className="App">
{element}
</div>
);
}
export default App;
```

- 安装 redux
  `npm i redux react-redux`
  新建 store/index.js、store/actions/test/index.js、store/reducers/test/index.js,
  在 store/index.js 中写入

```
import {createStore} from 'redux'
import test from './reducers/test/index.js'
export default createStore(test)
```

在 store/actions/test/index.js 中写入

```
import store from '../../index.js'
export const test1=(data)=>{
return store.dispatch( {type:'test1',data} )
}
```

在 store/reducers/test/index.js 中写入

```
const initState={}
export default function userInfo(preState=initState,action){
const {type,data}=action
console.log('xxxinfo',preState,action)
return preState
}
```

在 入口文件 src/index.js 中

```
import { BrowserRouter } from 'react-router-dom';
import { Provider } from 'react-redux';
import store from './store'
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
<React.StrictMode>
<Provider store={store}>
<BrowserRouter>
<App /></BrowserRouter>
</Provider>
</React.StrictMode>
);
```

- 设置代理
  在 src 中创建 setupProxy.js,在 react18 中使用

```
const proxy = require("http-proxy-middleware").createProxyMiddleware;
module.exports = function (app) {
app.use(
proxy("/apis", {
target: "http://xxx:xxx",
changeOrigin: true, //控制服务器收到的响应头中的 Host 字段值
//apis 前缀置空
pathRewrite: { "^/apis": "" },
}),
proxy("/api2", {
target: "http://xxx:xxx",
changeOrigin: true, //控制服务器收到的响应头中的 Host 字段值,true 时为代理服务 ip,false 为当前 ip，默认为 false
pathRewrite: { "^/api2": "" },
})
);
};
```

- 使用 sass
  安装：`npm install node-sass --save`
- 修改本地调试启动端口
  1.  依次打开“node_modules”- “react-scripts”-“scripts”文件夹，找到并打开 start.js 文件；在 start.js 文件中查找并修改“DEFAULT_PORT”项的值即可
  2.  修改 package.json 文件中

```
"scripts": {
"start": "set PORT=9000 && react-scripts start",
"build": "react-scripts build",
"test": "react-scripts test --env=jsdom",
"eject": "react-scripts eject"
}
```

- 修改 elint 语法检测
  修改 package.json 文件中

```

"eslintConfig": {
"extends": [
"react-app",
"react-app/jest"
],
"rules": {
"no-unused-vars": "off",
"no-undef": "off"
}
},

```

- 安装 antd
  `npm i antd --save`
  /src/index.js 入口文件中引入样式

```
import 'antd/dist/antd.min.css';
```

#### 创建后端项目

`npm install express-generator -g`
`express --view=ejs 项目名称`

- 启动项目

`http://dd

#### node 实例

**使用 nodejs 将 base64 文件转成图片和将图片转成 base64**

- 将 base64 转成 png 图片格式

```

    const fs = require('fs');
    const path = '你想要填写的路径/'+ Date.now() +'.png';
    const base64 = data.replace(/^data:image\/\w+;base64,/, "");//去掉图片base64码前面部分data:image/png;base64
    const dataBuffer = new Buffer(base64, 'base64'); //把base64码转成buffer对象，
    fs.writeFile(path, dataBuffer, function(err){//用fs写入文件
        if(err){
            console.log(err);
        }else{
            console.log('写入成功！');
        }
    })

```

- 将图片转成 base64

```

    const fs = require("fs");
    const util = require("util");
    const imageData = await util.promisify(fs.readFileSync(fileUrl)); // 例：xxx/xx/xx.png
    const imageBase64 = imageData.toString("base64");
    const imagePrefix = "data:image/png;base64,";
    console.log(imagePrefix + imageBase64);

```

### 常见错误

- 引入 antd 样式

```

Failed to parse source map: 'webpack://antd/./components/icon/style/index.less' URL is not supported

```

解决方法: antd-mobile 导入组件样式时加这个 min
`import 'antd/dist/antd.min.css'; `
