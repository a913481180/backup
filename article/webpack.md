---
title: Webpack
date: 2021-04-22 20:44:11
categories:
  - web
tags:
  - web
---

# Webpack

是一种前端资源构建工具，一个静态模块打包器（module bundler）。在 webpack 看来，前端所有资源文件都会作为模块处理，它将根据模块的依赖关系进行静态分析，打包成对应的静态资源（bundle）。

## 五个核心概念

### Entry

入口，指示 webpack 以哪个文件为入口起点开始打包，分析构建内部依赖图。

### Output

输出，指示 webpack 打包后的资源 bundles 输出到哪里去，以及如何命名

### Loader

让 webpack 能够去处理那些非 js 文件

### Plugins

用于执行范围更广的任务，如打包优化，压缩等

### Mode

指示 webpack 使用相应模式的配置

- development 本地开发模式

会将`process.env.NODE_ENV`的值设为 development;启用 NamedChunksPlugin 和 NamedModulesPlugin

开发环境：`webpack ./src/index.js -o ./build/built.js --mode=development`;webpack 会以./src/index.js 为入口文件开始打包，输出到./build/built.js

- production 上线生产模式

会将`process.env.NODE_ENV`的值设为 production;启用 FlagDependencyUsagePlugin,FlagIncludedChunksPlugin,ModuleConcatenationPlugin,NoEmitOnErrorsPlugin,OccurrenceOrderPlugin,SideEffectsFlagPlugin,UglifyJsPlugin；

生产环境：`webpack ./src/index.js -o ./build/built.js --mode=production`;webpack 会以./src/index.js 为入口文件开始打包，输出到./build/built.js

1. webpack 能处理 js/json 文件，不能处理 css、img 等其他文件
2. 生产环境和开发环境会将 es6 模块化编译成浏览器能识别的模块化
3. 生产环境比开发环境多一个压缩 js 代码的过程。

## 结构

- webpack.config.js

webpack 配置文件

```js
//用于拼接路径
const {resolve}=require('path');
//插件:需要下载
const HtmlWebpackPlugin=require('html-webpack-plugin');


//webpack配置
module.exports={
//入口起点
entry:'./src/index.js',

//输出
output:{
//输出文件名
filename:'built.js'
//输出路径
path: resolve(__dirname,'build')
},

//loader配置
module:{
rules:[ //loader配置

{
//匹配哪些文件
test:/\.css$/,
//使用哪些loader进行处理
use:[
//use数组中loader执行顺序：从右到左，从下到上，依次执行。
//创建style标签，将js中的样式资源插入进行，添加到head中生效
'style-loader',
//将css文件变成commonjs模块加载到jsa中,里面内容是样式字符串
'css-loader'
]
},

{
//匹配哪些文件
test:/\.less$/,
//使用哪些loader进行处理
use:[
'style-loader',
'css-loader',
//将less文件编译成css文件,需要下载less-loader和less
'less-loader'
]
},

{
//处理图片
test:/\.(jpg|png|gif)$/,
//问题：处理不了html中的img图片
//使用一个loader
loader:'url-loader',
options:{
limit:8*1024,  //图片小于8kb,会被处理成base64，减少请求数量，减轻服务器压力，但图片体积会更大
esModule:false,  //关闭url-loader的es6模块化，使用commonjs解析，避免解析时出现[object Module]
name:'[hash:10].[ext]',  //给图片重新命名，提取图片hash的前10位，扩展名为ext
}
},

{
//处理html中的img图片（负责引入img,从而能被url-loader处理）
test:/\.html$/,
loader:'html-loader'
},

{
//打包其他资源
exclude:/\.(css|js|html|less)$/, //排除css、js、html
loader:'file-loader',
opetions:{
name:'[hash:10].[ext]'
}
},

//语法检查,只检查自己写的源代码，第三方的库是不用检查的，检查规则需要在package.json中的eslintConsig中设置
//需下载eslint-config-airbnb-base   eslint-plugin-import
{
test:/\.js$/,
exclue:/node_modules/,
loader:'eslint-loader',
options{
fix:true //自动修复eslint的错误
}
},


//js兼容处理：babel-loader   @babel/core    @babel/preset-env
//基本js兼容性处理-->@babel/preset-env；只能转换基本语法，如promise不能转换
//全部js兼容性处理-->@babel/polyfill；会将所有兼容性代码全部引入，体积太大
//按需加载-->core-js
{
test:/\.js$/,
exclude:/node_modules/,
/*
当一个文件需要多个loader处理时，需要指定loader执行的先后顺序
*/
enforce:'pre', //优先执行
loader:'babel-loader',
options:{
//预设：指示babel做怎样的兼容处理
presets:[
'@babel/preset-env',
{
//按需加载
useBuiltIns:'usage',
///指定core-js版本
corejs:{
version:3
},
//指定兼容浏览器的版本
targets:{
chrome:'60',
firefox:'60',
edge:'60',
ie:'9'
}
},
]
}
},

]

/*//第二种写法
rules:[
{
test:/\.css$/,
use:[
//取代style-loader,提取js中的css成单独文件
MiniCssExtractPlugin.loader,
'css-loader',
/*
//css兼容性处理：帮postcss找到package.json中browserslist里面的配置，通过配置加载指定的css兼容性样式
//需下载postcss-loader  postcss-preset-env
loader:'postcss-loader',
options:{
ident:'postcss',
plugin:()=>[require('postcss-present-env')()]
},

*/
]
}
]
*/

},

//plugin配置
plugins:[

//html-webpack-plugin
//默认创建一个空的html，自动引入打包输出的所有文件
//需要有结构的html文件
new HtmlWebpackPlugin({
//复制‘./src/index.html’文件，并自动引入打包输出的所有文件资源
template:'./src/index.html',
minify:{ //压缩html代码
collapseWhitespace:true, //移除空格
removeComments:true  //移除注释
}
}),

/*
//css兼容
new MiniCssExtratPlugin({
filename:'css/built.css'
}),
//压缩css
new OptimizeCssAssetsWebpackPlugin()
*/

],

//模式
mode:'development'

//开启服务器devSever,用于自动编译，自动打开浏览器，刷新浏览器，但只在内存中编译打包，不就有实际输出
//启动指令为：npx webpack-dev-server
devServer:{
contentBase:resolve(__dirname,'build'),
//启用gzip压缩
compress:true,
//端口号
port:3333,
//自动打开浏览器
open:true,

}
}

```

- package.json

```json
...
"browserslist":{  //兼容的浏览器版本
//默认匹配生产环境，可通过修改process.env.NODE_ENV=developent来改变
 "developent":[
  "last 1 chrome version",
  "last 1 firefox version",
 ],
 "production":[
  ">0.2%",
  "not dead",
  "not op_mini all"
  ]
},

"eslintConfig":{
"extends":"airbnb-base"
},
```

- index.js

```js
//引用样式
import "./index.css";
//兼容js
import "@babel/polyfill";
```

## 性能优化

### 开发环境优化

- 打包构建速度

HMR:模块热替换;当模块发生变化时，只会重新打包这个模块，而不是打包全部模块。

package.json

```json
...
devServer:{
...
//开启HMR功能
hot:true
}
```

- 样式文件:可以使用 hmr 功能：style-loader 内部实现了
- js 文件：默认不能使用 hmr 功能,解决办法：修改 js 代码，添加支持 hmr 功能的代码，但只能处理非入口 js 文件的其他文件。
  index.html

```js
if (module.hot) {
  module.hot.accept("./test.js", function () {}); //监听test.js的变化，若变化则执行回调函数
}
```

- html 文件：默认不能使用 hmr 功能,解决办法：修改 entry，将 html 文件引入

webpack.config.js

```js
module.exports={
entry:['./src/js/index.js','./src/index.html'],
output:{...},
...
}
```

- 代码调试

### 生成环境优化

- 打包构建速度
- 代码运行的性能

source-map:一种提供源代码到构建后代码映射的技术，用于检测源代码的错误
种类：

> 内联和外部的区别：外部生成了文件而内联没有，内联的构建速度更快；

- source-map:外部
  错误代码准确信息和源代码的错误位置
- inline-source-map:内联
  只生成一个内联 source-map
  错误代码的准确信息和源代码的错误位置
- hidden-source-map:外部
  错误代码的错误原因，但没有错误位置
  不能追踪源代码错误，只能提示到构建后代码的错误位置
- eval-source-map:内联
  每一个文件都生成对应的 source-map，都在 eval
- nosources-source-map:外部
  错误代码的准确信息,但没有任何源代码信息
- cheap-souce-map:外部
  错误代码准确信息和源代码的错误位置,只能精确到行
- cheap-module-souce-map:外部
  错误代码准确信息和源代码的错误位置

开发环境：速度快(eval>inline>cheap>..)，调试友好(souce-map,cheap-module-souce-map,cheap-spuce-map)
所以应选择 eval-source-map/eval-cheap-module-souce-amp

生产环境：内联会让代码体积变大，所以不用内联，源代码要不要隐藏（nosource-source-map 全部隐藏，hidden-source-map 只隐藏源代码，会提示构建后代码错误信息）
所以选择 source-map/cheap-module-souce-map

### 生成环境优化 2

> soucemap 是需要的，soucemap 并不会影响运行时包体积，它只是个 souce mapping，在利用 sentry 等工具做错误监控时候如果没有 soucemap 将会陷入困境。换成非自家公司的 CDN 是一个极其危险的操作

#### 1.生产环境关闭 productionSourceMap、css sourceMap

众所周知，SourceMap 就是当页面出现某些错误，能够定位到具体的某一行代码，SourceMap 就是帮你建立这个映射关系的，方便代码调试。在生产环境中我们完全没必要开启这个功能（谁在生产环境调试代码？不会是你吧）
如下配置：

```js
const isProduction = process.env.NODE_ENV === "production";
// 判断是否是生产环境
module.exports = {
  productionSourceMap: !isProduction, //关闭生产环境下的SourceMap映射文件
  css: {
    sourceMap: !isProduction, // css sourceMap 配置
    loaderOptions: {
      ...其它代码,
    },
  },
  ...其它代码,
};
```

此时再`npm run build` 打包，就会发现速度快了很多，体积瞬间只有几兆了！

#### 2.分析大文件，找出内鬼

安装 `npm install webpack-bundle-analyzer -D` 插件，打包后会生产一个本地服务，清楚的展示打包文件的包含关系和大小。

vue.config.js 配置：

```js
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
module.exports = {
   ...其它
   configureWebpack: [
   plugins: [
      new BundleAnalyzerPlugin() // 分析打包大小使用默认配置
   ]
  },
 ...其它
}
```

自动弹出一个服务，清晰的展示打包后 js 的文件大小,把必须要用的第三方 js 通过 cdn 的方式引用。
分析发现，elementui、echarts 是必须使用的，打包又耗时且页面加载也较慢得很。可以通过 cdn 直接引入，方便且速度快。

#### 按需引入

element-ui 是我们项目用的主要框架，所以这个肯定是少不了，但是项目里面 ant-design 为什么会存在呢，原来是发现有个页面使用了 antd 的进度条组件，因为 elementui 的进度条不太好看。但是没想到这样把整个 antd 都导进来了。
方案：

1. 舍弃 antd 组件，自己去找一个类似的 vue 插件或者干脆自己实现一个。（这个方法短时间无法完成，且不想去动以前代码，暂不考虑）
2. 使用 antd 部分加载。只加载想要的进度条组件，可以减少文件体积（这个方法简单粗暴，就是牺牲一些文件大小）。

我们使用方案 2，根据 antd 官方的文档配置部分组件的引入。

安装 `npm install babel-plugin-import -D`

第一步 main.js 导入需要的组件 Step

```js
import { Steps } from "ant-design-vue";
Vue.component(Steps.name, Steps);
Vue.component(Steps.Step.name, Steps.Step);
```

第二步 babel.config.js 加上配置：

```js
module.exports = {
  presets: ["@vue/cli-plugin-babel/preset"],
  //以下是按需加载的配置++++
  plugins: [
    [
      "import",
      {
        libraryName: "ant-design-vue",
        libraryDirectory: "es",
        style: true,
      },
    ],
  ],
};
```

此时再分析，antd 已经小了很多。

#### CDN

项目里面第三方 js 很多，有些打包下来会很大，而且加载速度较慢。我们把这些 js 分离出来，通过 cdn 的方式在 html 中的 script 标签中直接使用，一方面减少打包体积，一方面提高了加载速度。
这里推荐一个免费的 cdn: BootCDN。也可以使用自己购买的付费 cdn 服务，我们到网站搜索自己项目需要的 js。
第一步：配置 vue.config.js，让 webpack 不打包这些 js，而是通过 script 标签加入。

```js
const isProduction = process.env.NODE_ENV === "production"; // 判断是否是生产环境
//正式环境不打包公共js
let externals = {};
//储存cdn的文件
let cdn = {
  css: [
    "https://cdn.bootcdn.net/ajax/libs/element-ui/2.15.0/theme-chalk/index.min.css", // element-ui css 样式表
  ],
  js: [],
};
//正式环境才需要
if (isProduction) {
  externals = {
    //排除打包的js
    vue: "Vue",
    "element-ui": "ELEMENT",
    echarts: "echarts",
  };
  cdn.js = [
    "https://cdn.bootcdn.net/ajax/libs/vue/2.6.11/vue.min.js", // vuejs
    "https://cdn.bootcdn.net/ajax/libs/element-ui/2.6.0/index.js", // element-ui js
    "https://cdn.bootcdn.net/ajax/libs/element-ui/2.6.0/locale/zh-CN.min.js",
    "https://cdn.bootcdn.net/ajax/libs/echarts/5.1.2/echarts.min.js",
  ];
}
module.exports = {
  //...其它配置
  configureWebpack: {
    //常用的公共js 排除掉，不打包 而是在index添加cdn，
    externals,
    //...其它配置
  },
  chainWebpack: (config) => {
    //...其它配置
    // 注入cdn变量 (打包时会执行)
    config.plugin("html").tap((args) => {
      args[0].cdn = cdn; // 配置cdn给插件
      return args;
    });
  },
  //...其它配置
};
```

第二步：html 模板中加入定义好的 cdn 变量使用的代码

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0" />
    <title>web</title>
    <link rel="icon" href="<%= BASE_URL %>favicon.ico" />
    <!-- 引入样式 -->
    <% for(var css of htmlWebpackPlugin.options.cdn.css) { %>
    <link rel="stylesheet" href="<%=css%>" />
    <% } %>

    <!-- 引入JS -->
    <% for(var js of htmlWebpackPlugin.options.cdn.js) { %>
    <script src="<%=js%>"></script>
    <% } %>
  </head>
  <body style="font-size:14px">
    <div id="app"></div>
  </body>
</html>
```

可以发现 cdn.js 中，我把 vue、echarts、element-ui 这三个大头加入了。在 externals 对象中左侧是 npm 包的名称，右侧是在代码中暴露的全局变量。注意 element-ui 对应的是 ELEMENT。

#### 懒加载的方式

没有使用 exclejs，是因为 exceljs 在我的业务代码中不是直接引用的，而是一个叫 table2excel 间接依赖的。所以就算我通过上面的方法排除掉它，在打包的时候还是会通过 table2excel 的依赖找到它并打包。
那这种不可避免的情况，该如何优化，让加载速度不受影响呢？

1.script 标签中注释掉 `import Table2Excel from "table2excel.js";`

2.下载的方法中：

```js
download(){
    //使用import().then()方式
    import("table2excel.js").then((Table2Excel) => {
        new Table2Excel.default("#table").export('filename') //多了一层default
    })
}
```

这样在进入系统时，不会加载 Table2Excel 和 exceljs，当需要时才会去加载，第一次会慢一点，后面就不需要加载了，会变快。

#### moment.js 的优化

我们发现 monentjs 在项目中有使用来对时间格式化，但是使用频率并不高，完全可以自己实现一个 format 方法，或者使用只有 6kb 的 day.js.

但这里我们暂不替换，把 moment 变得瘦小一些即可，删除掉除中文以外的语言包。
第一步：vue.config.js

```js
//...其它配置
chainWebpack: (config) => {
  config
    .plugin("ignore")
    //忽略/moment/locale下的所有文件
    .use(new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/));
};
//...其它配置
```

第二步：main.js

```js
import moment from "moment"; //手动引入所需要的语言包
import "moment/locale/zh-cn"; // 指定使用的语言
moment.locale("zh-cn");
```

#### gzip

通过 `compression-webpack-plugin` 插件把代码压缩为 gzip。但是！需要服务器支持
webpack 端 vue.config.js 配置如下：

```js
//打包压缩静态文件插件
const CompressionPlugin = require("compression-webpack-plugin");

//...其它配置
module.exports = {
  //...其它配置
  chainWebpack: (config) => {
    //生产环境开启js\css压缩
    if (isProduction) {
      config.plugin("compressionPlugin").use(
        new CompressionPlugin({
          test: /\.(js)$/, // 匹配文件名
          threshold: 10240, // 对超过10k的数据压缩
          minRatio: 0.8,
          deleteOriginalAssets: true, // 删除源文件
        })
      );
    }
  },
  //...其它配置
};
```

服务器端配置这里就不详细说明了可以谷百： nginx 开启静态压缩 找到答案。

## oneOf

```js
//以下loader只会匹配一个
//注意：不能有两个配置处理同一个类型文件
oneOf: [
  {
    test: /\.css$/,
    use: [...commonCssLoadder],
  },

  {},
];
```

---

1. webpack 打包后打开 index.html 无内容出现空白页面：修改 webpack 配置，在 util.js 中的`fallback:'vue-style-loader',`下面增加 publicPath:'../../'，修改 config/index.js，增加 build：{ assetsPublicPath:'./'}。另外，！路由模式改为 hash 模式
2. 界面正常显示但出现 cannot read property xxx of undefined（不能读取 xxx 属性）的问题：在渲染页面时，则会直接读取初始时的数据，若数据初始时为空，则容易出现错误。而请求是异步的，请求的数据会在页面首次渲染后才会发起

- webpack 设置服务器代理在 config 目录下的 index.js 中配置

1. 保留 eslint 的语法检测把不符合自己习惯的规则去掉，可配置文件在项目根目录里以 .eslintrc.\* 命名的文件。关闭方法，则是把 build/webpack.base.conf.js 配置文件中的 eslint rules 注释掉即可

## 常用 Loader（高频）

- raw-loader：加载文件原始内容（utf-8）

- file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)

- source-map-loader：加载额外的 Source Map 文件，以方便断点调试

- svg-inline-loader：将压缩后的 SVG 内容注入代码中

- image-loader：加载并且压缩图片文件

- json-loader 加载 JSON 文件（默认包含）

- babel-loader：把 ES6 转换成 ES5

- ts-loader: 将 TypeScript 转换成 JavaScript

- awesome-typescript-loader：将 TypeScript 转换成 JavaScript，性能优于 ts-loader

- sass-loader：将 SCSS/SASS 代码转换成 CSS

- css-loader：加载 CSS，支持模块化、压缩、文件导入等特性

- style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS

- postcss-loader：扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀

- vue-loader：加载 Vue.js 单文件组件

## 常用的 Plugin

- define-plugin：定义环境变量 (Webpack4 之后指定 mode 会自动配置)

- ignore-plugin：忽略部分文件

- html-webpack-plugin：简化 HTML 文件创建 (依赖于 html-loader)

- web-webpack-plugin：可方便地为单页应用输出 HTML，比 html-webpack-plugin 好用

- uglifyjs-webpack-plugin：不支持 ES6 压缩 (Webpack4 以前)

- terser-webpack-plugin: 支持压缩 ES6 (Webpack4)

- webpack-parallel-uglify-plugin: 多进程执行代码压缩，提升构建速度

- mini-css-extract-plugin: 分离样式文件，CSS 提取为独立文件，支持按需加载 (替代 extract-text-webpack-plugin)

- serviceworker-webpack-plugin：为网页应用增加离线缓存功能

- clean-webpack-plugin: 目录清理

## Loader 和 plugin 的区别

Loader 本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。 因为 Webpack 只认识 JavaScript，所以 Loader 就成了翻译官，对其他类型的资源进行转译的预处理工作。

Plugin 就是插件，基于事件流框架 Tapable，插件可以扩展 Webpack 的功能，在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

Loader 在 module.rules 中配置，作为模块的解析规则，类型为数组。每一项都是一个 Object，内部包含了 test(类型文件)、loader、options (参数)等属性。

Plugin 在 plugins 中单独配置，类型为数组，每一项是一个 Plugin 的实例，参数都通过构造函数传入
