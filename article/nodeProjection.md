---
title: 全栈项目
date: 2022-01-12 20:11:33
categories:
  - web
---

# 全栈

## 初始化

### 创建前端项目

#### vue

##### 创建项目

`vue create 项目名称`

##### 使用 Vue Styleguidist 编写组件文档

[Vue Styleguidist官网](https://vue-styleguidist.github.io/)

首先，Vue Styleguidist 只能适用于 Webpack 打包的项目，在此基础上，我们需要安装 vue-styleguidist 这个包

`npm install vue-styleguidist --save-dev`
然后在 package.json 配置下面两行命令，分别用于开发预览和部署打包

```
{
  "scripts": {
    "styleguide": "vue-styleguidist server",
    "styleguide:build": "vue-styleguidist build"
  }
}
```

如果是使用 @vue/cli 3 生成的项目，可以直接使用 `vue-cli-plugin-styleguidist` 这个插件进行更快捷的安装和配置

- Props
  Props 是组件最基本的 API，用于为组件传递数据,实际上，在配置好 Vue Styleguidist 之后，如果有写 prop，就已经能生成一个这样的文档
- Events
  除了 Props，Event 事件也是 Vue 的一个重要的 API 之一，可以通过 v-on 为组件绑定事件。Vue 的事件使用 vm.$('event', ...params) 的方法进行定义，我们只需要在这个方法之前，加上必要的注释就可以了。如果事件名不是字符串，可以使用 @event 进行标注，事件的参数使用 @type 进行标注

  ```
  export default {
  methods: {
    handleClick(e) {
      /**
       * 单击事件
       * @type {Event}
       */
      this.$emit('click', e);
    },
    },
    };
  ```

- Slots
  Slot 插槽是 Vue 的自定义元素之一，Slot 向一个组件传递内容，也是封装公共组件常见的 API 之一,与 Props 和 Events 不同的是，Slots 通常是定义在`<template>`部分，不能使用 JS 注释进行标注，需要使用 HTML 注释，并且在注释里使用 @slot 进行标注

  ```
    <button
  class="btn"
  :type="htmlType"
  :class="btnClass"
  :disabled="disabled"
  @click="handleClick"
    >
  <!-- @slot 按钮的内容 -->
  <slot />
  </button>
  ```

- Methods
  看到这里可能各位会有个疑问，Methods 和 Events 有什么区别？区别主要有以下两个：定义方式不同：Methods 只要在 methods 里定义函数即可，Events 则需要使用 `vm.$('event', ...params)` 进行定义,调用方式不同：Methods 使用`vm.$refs.ref.method()`这样的方式进行调用，Events 使用 v-on 指令或者 `vm.$on('event')` 进行监听。实际上，使用 Methods 方法封装组件 API 的情况是比较少的，但是依然不能排除这种情况。对于 Methods 方法，我们只需要像使用 JSDoc 一样为函数进行注释就可以了，最后再附上 @public 进行标识

  ```
  export default {
  methods: {
    /**
     * 单击事件
     * @param {Event} e
     * @public
     */
    click(e) {
      // some code
    },
  },};
  ```

- 组件样例
  README 方式,在组件的同个目录下，新建一个文件 `README.md`，比如 `src/components/AppButton/README.md`然后直接调用组件代码
  可以直接在组件下方增加一个组件 `<docs></docs>`，并在里面直接使用 Markdown 编写样例，这种方式适用于项目结构比较简单的项目，但是我认为会使组件的代码变得很冗长，在此不再赘述

- 配置
  Vue Styleguidist 支持自定义配置，只需要在项目根目录下，创建 styleguide.config.js，就可以参照官方文档进行配置

  ```
  // styleguide.config.js
  module.exports = {
  title: 'Default Style Guide',          // 文档的标题
  components: 'src/components/**/*.vue', // 组件的目录
  defaultExample: false,                 // 是否使用默认样例
  usageMode: 'expand',                   // 是否展开用法
  exampleMode: 'expand',                 // 是否展开示例代码
  styleguideDir: 'styleguide',           // 打包的目录
  codeSplit: true,                       // 打包时是否进行分片
  skipComponentsWithoutExample: true,    // 是否跳过没有样例的组件
  };
  ```

- 注意
  JSDoc 的标签仍然有效。TypeScript、Flow 和 Class 组件同样可以使用，只是使用方法稍有不同。JSX 也可以使用。实际上 Vue Styleguidist 来源于 React Styleguidist（也依赖于这个项目），使用方法大同小异，不同于 Vue 组件的是，React 组件的 API 只有 Props 一项

#### react

##### 安装脚手架

`npm i create-react-app -g`

##### 创建文件夹并初始化

`create-react-app test_project`
`cd test_project`
`npm run start`

使用ts

`create-react-app demo --template typescript`
`cd test_project`
`npm run start`

##### 安装路由

`npm i react-router-dom`
创建 routes/index.js 文件,写入路由表

```js
import { lazy, Suspense,  } from "react";
import { Navigate } from "react-router-dom";
import LoadingPage from "../pages/LoadingPage";
const Home = lazy(() => import("../pages/Home"));
const ErrorBlock = lazy(() => import("../pages/ErrorBlock"));
const routes= [
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
export default routes
```

入口文件 index.js 中加入

```js
import {BrowserRouter} from 'react-router-dom'
<BrowserRouter>
<App/>
</BrowserRouter>
```

    App.js 中引入路由表

```jsx
import './App.scss';
import routes from './routes/'
import { useRoutes } from 'react-router-dom'
function App() {
  const element = useRoutes(routes)
    return (<div className="App">{element}</div>);
}
export default App;
```

##### 安装 redux

`npm i redux react-redux`
新建 store/index.js、store/actions/test/index.js、store/reducers/test/index.js,
在 store/index.js 中写入

```js
import {createStore} from 'redux'
import test from './reducers/test/index.js'
export default createStore(test)
```

在 store/actions/test/index.js 中写入

```js
import store from '../../index.js'
export const test1=(data)=>{
return store.dispatch( {type:'test1',data} )
}
```

在 store/reducers/test/index.js 中写入

```js
const initState={}
export default function userInfo(preState=initState,action){
const {type,data}=action
console.log('xxxinfo',preState,action)
return preState
}
```

在 入口文件 src/index.js 中

```js
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

##### 设置代理

在 src 中创建 setupProxy.js,在 react18 中使用

```js
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

##### 使用 sass

安装：`npm install node-sass --save-dev`

##### 使用 less

安装：`npm install less less-loader -D`
提交代码

```bash
git add .
git commit -am 'xxxx'
```

暴露webpack配置文件`npm run  eject`

修改webpack.config.js，拷贝sass的配置改写为less

```js
//....
const lessRegex = /\.(less)$/;
const lessModuleRegex = /\.module\.(less)$/;
//....
```

```js
//...
        {
              test: lessRegex,
              exclude: lessModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 3,
                  sourceMap: isEnvProduction
                    ? shouldUseSourceMap
                    : isEnvDevelopment,
                  modules: {
                    mode: "icss",
                  },
                },
                "less-loader"
              ),
              // Don't consider CSS imports dead code even if the
              // containing package claims to have no side effects.
              // Remove this when webpack adds a warning or an error for this.
              // See https://github.com/webpack/webpack/issues/6571
              sideEffects: true,
            },
            // Adds support for CSS Modules, but using SASS
            // using the extension .module.less 
            {
              test: lessModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 3,
                  sourceMap: isEnvProduction
                    ? shouldUseSourceMap
                    : isEnvDevelopment,
                  modules: {
                    mode: "local",
                    getLocalIdent: getCSSModuleLocalIdent,
                  },
                },
                "less-loader"
              ),
            },
//...
```

修改react-app-env-d.ts文件，加入

```ts
//...
declare module '*.module.less' {
  const classes: { readonly [key: string]: string };
  export default classes;
}
```

##### 修改本地调试启动端口

1. 依次打开“node_modules”- “react-scripts”-“scripts”文件夹，找到并打开 start.js 文件；在 start.js 文件中查找并修改“DEFAULT_PORT”项的值即可
2. 修改 package.json 文件中

```json
"scripts": {
"start": "set PORT=9000 && react-scripts start",
"build": "react-scripts build",
"test": "react-scripts test --env=jsdom",
"eject": "react-scripts eject"
}
```

##### 修改 elint 语法检测

修改 package.json 文件中

```json

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

##### 安装 antd

`npm i antd --save`
/src/index.js 入口文件中引入样式

```js
import 'antd/dist/antd.min.css';
```

##### eslint

安装执行`yarn add -D eslint`

项目目录添加.eslintignore，忽略一些不需要 eslint 检测的目录和文件

```txt
.eslintrc.js
node_modules
dist
.DS_Store
*.local
```

至此，我们可以在package.json添加命令

```json
{
  "script": {
    "lint:js": "eslint --ext .js,.jsx,.ts,.tsx ./src"
  }
}
```

#### 安装 prettier

`yarn add -D prettier`

项目目录添加.prettierrc.js文件，我们添加常用的格式风格

```js

module.exports = {
  "printWidth": 80, //一行的字符数，如果超过会进行换行，默认为80
  "tabWidth": 2, //一个tab代表几个空格数，默认为80
  "useTabs": false, //是否使用tab进行缩进，默认为false，表示用空格进行缩减
  "singleQuote": false, //字符串是否使用单引号，默认为false，使用双引号
  "semi": true, //行位是否使用分号，默认为true
  "trailingComma": "none", //是否使用尾逗号，有三个可选值"<none|es5|all>"
  "bracketSpacing": true, //对象大括号直接是否有空格，默认为true，效果：{ foo: bar }
  //"parser": "babylon" //代码的解析引擎，默认为babylon，与babel相同。
}

```

项目目录添加.prettierignore，忽略一些不需要 prettierg 格式化的文件

```txt
**/*.png
**/*.svg
package.json
dist
.DS_Store
node_modules
```

至此，我们可以在package.json添加命令

```json
{
  "script": {
    "lint:prettier": "prettier -c --write \"src/**/*\""
  }
}
```

避免 eslint 和 prettier 冲突，我们需要再安装两个包
`yarn add -D eslint-config-prettier eslint-plugin-prettier`
安装后我们只需要在.eslintrc.js文件添加一行即可

```js
//...
{
  extends: [
    // ...
    'plugin:prettier/recommended'
  ]
}
```

修改规则,修改 .eslintrc.js 文件

```js
  rules: {
    //...
    'prettier/prettier': [
      'error',
      {
        singleAttributePerLine: false, // 不强制要求一个属性占一行
      }
    ]
    //....
  }

```

`eslint-config-prettier`的作用是关闭 eslint 中所有不必要的或可能与 prettier 冲突的规则，让 eslint 检测代码时不会对这些规则报错或告警。
`eslint-plugin-prettier`的作用是将 prettier 作为 ESLint 的规则来使用，相当于代码不符合 Prettier 的标准时，会报一个 ESLint 错误，同时也可以通过 eslint --fix 来进行格式化。

#### 安装 stylelint

检测 css 样式代码质量，其实很多项目都是不检测的，如果不做这步可以忽略。
`yarn add -D stylelint stylelint-config-standard`
项目目录中添加.stylelintrc.js文件

```js
module.exports = {
  extends: ['stylelint-config-standard'],
};
```

我们使用了官方推荐的stylelint-config-standard配置就好

至此，我们可以在package.json添加命令

```js
{
  "script": {
    "lint:style": "stylelint \"**/*.css\""
  }
}
```

同样的，我们统一用 prettier 来格式化 css 代码。 需要安装stylelint插件来避免与prettier冲突。
`stylelint-config-prettier`，作用是关闭 stylelint 所有不必要的或可能与 prettier 冲突的规则。但是在 Stylelint v15 版本之后，Stylelint 默认关闭了所有与 prettier 相冲突的风格规则，所以不需要安装`stylelint-config-prettier`了。
`stylelint-prettier`，开启了以 prettier 为准的规则，并将报告错误给 stylelint
修改.stylelintrc.js文件

```js
module.exports = {
  extends: ['stylelint-config-standard', 'stylelint-prettier/recommended'],
};
```

我们如果使用vscode开发，一定要安装ESlint、Stylelint、Prettier这三个插件，只要项目安装了ESlint和Stylelint包，那么Vscode可以实时把代码质量问题报红提示，对于强迫症的人，肯定想及时把报红消灭

### 创建后端项目

`npm install express-generator -g`
`express --view=ejs 项目名称`

- 启动项目

`<http://dd>

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
`import 'antd/dist/antd.min.css';`
