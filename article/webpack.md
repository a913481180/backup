---
title: Webpack
date: 2021-04-22 20:44:11
categories:
- web
---

# Webpack

是一种前端资源构建工具，一个静态模块打包器（module bundler）。在webpack看来，前端所有资源文件都会作为模块处理，它将根据模块的依赖关系进行静态分析，打包成对应的静态资源（bundle）。

## 五个核心概念

### Entry

入口，指示webpack以哪个文件为入口起点开始打包，分析构建内部依赖图。

### Output

输出，指示webpack打包后的资源bundles输出到哪里去，以及如何命名

### Loader

让webpack能够去处理那些非js文件

### Plugins

用于执行范围更广的任务，如打包优化，压缩等

### Mode

指示webpack使用相应模式的配置

- development本地开发模式

会将`process.env.NODE_ENV`的值设为development;启用NamedChunksPlugin和NamedModulesPlugin

开发环境：`webpack ./src/index.js -o ./build/built.js --mode=development`;webpack会以./src/index.js为入口文件开始打包，输出到./build/built.js

- production上线生产模式

会将`process.env.NODE_ENV`的值设为production;启用FlagDependencyUsagePlugin,FlagIncludedChunksPlugin,ModuleConcatenationPlugin,NoEmitOnErrorsPlugin,OccurrenceOrderPlugin,SideEffectsFlagPlugin,UglifyJsPlugin；

生产环境：`webpack ./src/index.js -o ./build/built.js --mode=production`;webpack会以./src/index.js为入口文件开始打包，输出到./build/built.js

1. webpack能处理js/json文件，不能处理css、img等其他文件
2. 生产环境和开发环境会将es6模块化编译成浏览器能识别的模块化
3. 生产环境比开发环境多一个压缩js代码的过程。

## 结构

- webpack.config.js

webpack配置文件

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
import './index.css'
//兼容js
import '@babel/polyfill'
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

- 样式文件:可以使用hmr功能：style-loader内部实现了
- js文件：默认不能使用hmr功能,解决办法：修改js代码，添加支持hmr功能的代码，但只能处理非入口js文件的其他文件。
index.html

```js
if(module.hot){
module.hot.accept('./test.js',function(){}); //监听test.js的变化，若变化则执行回调函数
}
```

- html文件：默认不能使用hmr功能,解决办法：修改entry，将html文件引入

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

>内联和外部的区别：外部生成了文件而内联没有，内联的构建速度更快；

- source-map:外部
错误代码准确信息和源代码的错误位置
- inline-source-map:内联
只生成一个内联source-map
错误代码的准确信息和源代码的错误位置
- hidden-source-map:外部
错误代码的错误原因，但没有错误位置
不能追踪源代码错误，只能提示到构建后代码的错误位置
- eval-source-map:内联
每一个文件都生成对应的source-map，都在eval
- nosources-source-map:外部
错误代码的准确信息,但没有任何源代码信息
- cheap-souce-map:外部
错误代码准确信息和源代码的错误位置,只能精确到行
- cheap-module-souce-map:外部
错误代码准确信息和源代码的错误位置

开发环境：速度快(eval>inline>cheap>..)，调试友好(souce-map,cheap-module-souce-map,cheap-spuce-map)
所以应选择eval-source-map/eval-cheap-module-souce-amp

生产环境：内联会让代码体积变大，所以不用内联，源代码要不要隐藏（nosource-source-map全部隐藏，hidden-source-map只隐藏源代码，会提示构建后代码错误信息）
所以选择source-map/cheap-module-souce-map

## oneOf

```js
//以下loader只会匹配一个
//注意：不能有两个配置处理同一个类型文件
oneOf:[
{
test:/\.css$/,
use:[...commonCssLoadder]
},

{}
]
```

---

1. webpack打包后打开index.html无内容出现空白页面：修改webpack配置，在util.js中的`fallback:'vue-style-loader',`下面增加publicPath:'../../'，修改config/index.js，增加build：{ assetsPublicPath:'./'}。另外，！路由模式改为hash模式
2. 界面正常显示但出现cannot read property xxx of undefined（不能读取xxx属性）的问题：在渲染页面时，则会直接读取初始时的数据，若数据初始时为空，则容易出现错误。而请求是异步的，请求的数据会在页面首次渲染后才会发起

- webpack设置服务器代理在config目录下的index.js中配置

1. 保留eslint的语法检测把不符合自己习惯的规则去掉，可配置文件在项目根目录里以 .eslintrc.* 命名的文件。关闭方法，则是把 build/webpack.base.conf.js 配置文件中的eslint rules注释掉即可


## 常用Loader（高频）
- raw-loader：加载文件原始内容（utf-8）

- file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)

- source-map-loader：加载额外的 Source Map 文件，以方便断点调试

- svg-inline-loader：将压缩后的 SVG 内容注入代码中

- image-loader：加载并且压缩图片文件

- json-loader 加载 JSON 文件（默认包含）

- babel-loader：把 ES6 转换成 ES5

- ts-loader: 将 TypeScript 转换成 JavaScript

- awesome-typescript-loader：将 TypeScript 转换成 JavaScript，性能优于 ts-loader

- sass-loader：将SCSS/SASS代码转换成CSS

- css-loader：加载 CSS，支持模块化、压缩、文件导入等特性

- style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS

- postcss-loader：扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀

- vue-loader：加载 Vue.js 单文件组件

## 常用的Plugin
- define-plugin：定义环境变量 (Webpack4 之后指定 mode 会自动配置)

- ignore-plugin：忽略部分文件

- html-webpack-plugin：简化 HTML 文件创建 (依赖于 html-loader)

- web-webpack-plugin：可方便地为单页应用输出 HTML，比 html-webpack-plugin 好用

- uglifyjs-webpack-plugin：不支持 ES6 压缩 (Webpack4 以前)

- terser-webpack-plugin: 支持压缩 ES6 (Webpack4)

- webpack-parallel-uglify-plugin: 多进程执行代码压缩，提升构建速度

- mini-css-extract-plugin: 分离样式文件，CSS 提取为独立文件，支持按需加载 (替代extract-text-webpack-plugin)

- serviceworker-webpack-plugin：为网页应用增加离线缓存功能

- clean-webpack-plugin: 目录清理

## Loader 和 plugin 的区别
Loader 本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。 因为 Webpack 只认识 JavaScript，所以 Loader 就成了翻译官，对其他类型的资源进行转译的预处理工作。

Plugin 就是插件，基于事件流框架 Tapable，插件可以扩展 Webpack 的功能，在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

Loader 在 module.rules 中配置，作为模块的解析规则，类型为数组。每一项都是一个 Object，内部包含了 test(类型文件)、loader、options (参数)等属性。

Plugin 在 plugins 中单独配置，类型为数组，每一项是一个 Plugin 的实例，参数都通过构造函数传入

