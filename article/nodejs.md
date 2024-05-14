---
title: NodeJs
date: 2021-06-12 20:11:33
categories:
  - web
---

# NodeJs

## Node.js 模块化开发

Node.js 规定一个 Javascript 文件就是一个模块，模块内部定义的变量和函数默认情况下外部无法得到。
模块内部可以使用 exports 对象进行成员导出，使用 require 方法导入其他模块。

### 模块成员导出

```js
//a.js
//在模块中定义变量
let version = 1.0;
//模块中定义方法
const sayHi = (name) => `hello！${name}`; //es6新语法
//向模块外部导出数据
exports.version = version;
exports.sayHi = sayHi;
```

### 模块成员的导入

```js
//b.js
//在b.js模块中导入模块a
let a = require("./b.js");
//输出b模块中的version变量
console.log(a.version);
//调用b模块中的sayHi方法
console.log(a.sayHi("XiaoMing"));
```

模块成员导出的另一种方法

```js
module.exports.version = version;
module.exports.sayHi = sayHi;
```

exports 是 module.exports 的别名，导出对象最终以 module.exports 为准；

## package.json 文件的作用

项目描述文件，记录了当前项目信息。如：项目版本、作者、依赖的模块等等；使用`npm init -y`生成。其作用为（一）锁定包的版本，确保再次下载时不会因为包版本不同而产生问题。（二）加快下载速度，其记录着依赖的下载地址。

- 开发依赖

在项目开发阶段所需要的依赖，线上运营阶段不需要依赖的第三方包,称为开发依赖；可使用`npm install 包名 --save-dev`将包添加到 package.json 文件的 devDependencies 字段中

```json
{
  "devDependencies": {
    "gulp": "^3.9.1"
  }
}
```

## Node.js 中模块的加载机制

模块查找规则

- 当模块有路径没后缀时

```node
require('./find.js);
require('./find');
```

1. require 方法根据路径查找模块
2. 如果模块后缀省略，则先找同名 JS 文件再找同名 JS 文件夹
3. 如果找到同名文件夹，则继续寻找其中的 index.js 文件
4. 如果没有 index.js 文件，就会去当前文件夹中的 package.js 文件中查找 main 选项中的入口文件
5. 如果入口文件不存在或没有指定入口文件，则报错：模块未找到

- 当模块即没路径也没后缀时

```node
require("find");
```

1. Node.js 会假设它是系统模块，会去 node_modules 文件中寻找
2. 先查看是否有同名 js 文件
3. 在看有没有同名文件夹
4. 若有，则继续寻找其中的 index.js 文件
5. 如没有，则查看该文件夹中的 package.json 中的 main 选项确定模块的入口文件
6. 若都没有，则报错

## 系统模块:Node 运行环境提供的 API

### fs

#### fs 文件操作

`const fs=require('fs');`

读取文件内容:`fs.readFile('文件路径/文件名'[,'文件编码'],callback);`(括号内为可选项)

实例：

```js
//读取上一级css目录下的base.css
fs.readFile("../css/base.css", "utf-8", (err, doc) => {
  //如果读取错误参数err的值为错误对象，否则err的值为null
  //doc参数为文件内容
  if (err == null) {
    //控制台中输出文件内容
    console.log(doc);
  }
});
```

写入文件内容：`fs.writeFile('文件路径/文件名称','数据',callback);

实例：

```js
const conetent = "<h3>!!!!!!!!!!</h3>";
fs.writeFile("../index.html", content, (err) => {
  if (err != null) {
    console.log(err);
    return;
  }
  console.log("文件写入成功");
});
```

```js
fs.stat(file, (err,data)=>{}) //查询文件状态（data.isFile()–判断是否是文件，isDirectory()–判断是否是文件夹）
fs.mkdir(fileName,err => {})// 创建文件夹 （fileName–文件夹名称，重复会报错，例如：’./test‘），注意：此方法不能连续创建文件夹，例如：/test/one/two
fs.readdir(fileName,err => {})//读取文件夹，返回文件夹的所有直系子文件名称，移动文件位置等
fs.rename(newFileName,oldFileName,err => {})//修改文件夹、文件的名称
fs.writeFile(file,err=> {})// 创建文件 （file—应路径下的文件,包括文件名，例如：’./test/test.txt‘ ，如文件已存在，会覆盖原文件内容）
fs.appendFile(file,err=> {})// 追加文件内容，会在文件内容末尾追加（若文件不存在，回先自动创建，类似于writeFile）
fs.readFile(file,’utf-8‘，(err,data)=>{})// 读取文件内容，不设置格式，会返回Buffer数据，需要设置格式，例如utf-8，或者通过data.toString()进行转化
fs.unlink(file,err=> {})// 删除文件
fs.rmdir(file,err=> {}) //删除文件夹，若文件夹下有文件，会报错，只能删除空文件夹
//注：若要连续创建文件夹，可以下载mkdirp模块包，mkdirp('./one/two/three') 既可以创建连续目录。（目前测试不支持创建文件，写入内容，npm地址:https://www.npmjs.com/package/mkdirp）

```

#### fs 流操作

- 写入流 createWriteStream
- 读取流 createReadStream
- 管道流 pipe

```js
writeStr: (str) => {
  // 需要先创建流
  const writeStream = fs.createWriteStream("./test.txt");
  writeStream.write(str); // str为写入内容
  writeStream.end(); // 此句必须写，不然无法触发下面的finish事件
  writeStream.on("finish", () => {
    console.log("写入完成");
  });
};

//------------------------------------------------------------------------------------------------------------/-------/

let str = "";
let count = 0;
let readStream = fs.createReadStream("./test.txt");
console.log(readStream);
readStream.on("data", (data) => {
  str += data;
  count++;
});
readStream.on("end", () => {
  console.log(str, "str----", count);
});
readStream.on("err", (err) => {
  console.log(err, "err----");
});

//------------------------------------------------------------------------------------------------------------/-------/

let readStream = fs.createReadStream("./test.txt");
let writeStream = fs.createWriteStream("./one.txt");
// 将读取的test文件内容，写入到one，one如有内容，会被覆盖
readStream.pipe(writeStream);
```

### path 路径拼接

`path.join('路径','路径',..);`

```js
//导入path模块
const path=require('path');
//路径拼接
let finialPath=path.join('itcast','a,'css','test.css');
//输出
console.log(finialPath);
//用__dirname获取当前文件的绝对路径
const fs=require('fs');
fs.readFile(path.join(__dirname,'test.css'),'utf-8',(err,doc)=>{
console.log(err);
console.log(doc);
});
```

### http

#### HTTP 协议

超文本传输协议。规定了客户端（浏览器）与服务器（网站服务器）之间请求和应答的标准

- 静态资源：服务器不需要处理，可以直接响应给客户端的资源，如：css，JavaScript，image 文件
- 动态资源：相同的请求地址，不同的响应资源

#### web 服务器

```js
//引用系统模块
const http = require("http");
//创建web服务器
const app = http.createServer();
//当客户端发送请求的时候
app.on("request", (req, res) => {
  //响应

  res.end("hello");

  //返回状态码和返回资源类型,若无，低级浏览器可能出问题

  res.writeHead(400, { "content-type": "text/html;charset=utf8" });
});

//监听端口
app.listen(3000);
console.log("Server is running....");
```

#### 路由

指客户端请求地址,如：`http://localhost:8080/index`与服务端程序代码的对应关系，即请求什么响应什么；

```js
app.on("request", (req, res) => {
  //获取客户端的请求路径
  let { pathname } = url.parse(req.url);
  //或写成
  //let pathname=url.parse(req.url).pathname;
  if (pathname == "/" || pathname == "/index") {
    res.end("欢迎来到首页");
  } else {
    res.end("页面不存在");
  }
});
```

#### 报文

在 HTTP 请求和响应的过程中传递的数据块叫做报文

- 请求报文

  1. 请求方式：GET、POST
  2. 请求地址
  3. 请求参数

  ```js
  app.on("request", (req, res) => {
    req.headers; //请求报文
    req.url; //请求地址
    req.method; //请求方法
  });
  ```

- 响应报文

  1. HTTP 状态码： 200(请求成功)、400(客户端请求语法有误)、404(请求的资源未找到)、500(服务器错误)、
  2. 内容类型：
     - text/html
     - text/css
     - application/javascript
     - image/jpeg
     - application/json

#### 完善：

```js
//引用系统模块
const http = require("http");
const url = require("url"); //处理url地址模块
const querystring = require("querystring"); //处理参数模块

//创建web服务器
const app = http.createServer();

//当客户端发送请求的时候
app.on("request", (req, res) => {
  //post参数是通过事件的方式接受的
  //data当请求参数传递的时候发出data事件
  //end当参数传递完成的时候时发出end事件
  //获取POST参数需要使用data事件和end事件；
  let postParams = "";
  req.on("data", (params) => {
    postParams += params; //接收的数据不是一次性发完的
  });
  req.on("end", () => {
    console.log(postParams);
    console.log(querystring.parse(postParams)); //将参数转换为对象格式
  });

  //响应
  res.end("hello");

  //返回状态码和相关信息

  res.writeHead(400, { "content-type": "text/html;charset=utf8" });

  //调用url模块方法parse();第一个参数为要处理的地址，第二个参数为是否处理为对象格式，true为是；
  console.log(url.parse(res.url, true));
});

//监听端口
app.listen(3000);
console.log("Server is running....");
```

## 第三方模块

npm(node package manager):node 的第三方模块管理工具

- 下载：`npm install 模块名`
- 卸载：`npm uninstall package 模块名`

全局安装和本地安装：一般命令行工具全局安装，库文件本地安装

### Express 框架

基于 node 平台的 web 应用开发框架，可使用`npm install express`下载

```js
//引用框架
const express=require('express');
//创建网站服务器
const app=express();

//当客户端以get方式访问/时
app.get('/',(req.res)=>{
//获取get参数
console.log(req.query);
//响应:sen方法会自动设置http状态码，响应头部,类型
res.send('hello!!!!');
});
//当客户端以post方式访问/add时
app.post('/add',(req.res)=>{
//获取post参数：需要第三方模块body-parser
console.log(req.body);
res.send('hello!!!!');
});

app.listen(3000);
console.log('server is runningn ...');
```

#### 中间件

> 将客户端发来的请求进行拦截处理

如：

```js
app.get("请求地址", "处理函数");
app.post("请求地址", "处理函数");
```

针对一个请求可设置多个中间件：

```js
app.get("/", (req, res, next) => {
  //next方法：该中间件处理后会将请求的控制权交给下一个中间件
  req.name = "xiaoming";
});
app.get("/", (req, res) => {
  console.log(req.name);
});
```

- app.use 中间件用法

会匹配所有的请求方法,直接传入请求处理函数代表接收所有请求

```js
app.use((req, res) => {
  console.log(req.url);
  next();
});
```

- 用于错误处理

```js
app.get('/index',(req,res)=>{
 throw new Error('程序发生未知错误'); //易出现错误的地方
});
app.use((err,req,res,next)=>{
 res.status(500).send(err.message); //只能用于处理同步代码错误
});

//异步代码错误需要手动触发错误
app.get('/index',(req,res,next)=>{
 fs.readFile('./test.c','utf-8',(err,data)=>{
 if(err){
 next(err); //易出现错误的地方
}
}});
app.use((err,req,res,next)=>{
 res.status(500).send(err.message); //只能用于处理同步代码错误
});


```

- 静态资源的处理

通过 express 内置的 express.static 托管静态文件

```js
app.use(express.static("./pulic"));
```

#### body-parser 中间件

> 处理用户 post 请求提交的数据，把数据保存在 req.body 中
> 此中间件已经被 express 集成，无需调用安装 body-parser，可以直接采用 express.json()和 express.urlencoded()实现相同功能。
> Express v4.16.0 引入了 express.json()、express.urlencoded() 中间件，express.json() 可解析 json 类型的 req.body，express.urlencoded() 可解析 urlencoded 类型的 req.body；
> Express v4.17.0 又引入 express.raw() 、express.text() 中间件，express.raw() 可解析 raw 类型的 req.body（解析为 Buffer）， express.text() 可解析 text 类型的 req.body（解析为 String）；

```js
const bodyParser = require("body-parser");
//配置body-parser模块
app.use(bodyParser.urlencoded({ extender: false }));
//接收请求
app.post("/add", (req, res) => {
  console.log(req.body);
});
```

- `bodyParser.json([options])`
  解析并返回 json 格式的数据，这是常用的方法。内部会查看 content-type，只有是正确的 content-type 默认是 application/json 才进入这个中间件解析处理。

```js
{
  // default = true
  // 是否开启压缩体解析
  "inflate": true,

  // default = '100kb'
  // 最大请求数据，传入数字默认单位是bytes，传入字符串要带上单位
  "limit": "100kb",

  // 指导reviver就相当于在JSON.parse()方法传入了第二个参数reviver做数据的预处理。
  "reviver": (key, value)=> {...},

   // default = true
   // 开启严格模式只能接收能被JSON.parse()方法解析的数据
   "strict": true,

   // 接收数据的类型，默认是"application/json"
   "type": "application/json",

   // 验证数据，如果无效就可以提前抛出错误信息
   "verify": (req, res, buf, encoding) => {...}
}
```

- `bodyParser.urlencoded([options])`
  这是常用的方法，常见的前端请求解决方案如表单 post 提交、axios、fetch 等库的 post 请求都需要这个中间件进行解析，返回 json 的格式数据。当请求的数据类型是 application/x-www-form-urlencoded 时才会进入这个中间件进行处理。

参数

```js
{
  // default = true
  // 解析URL-encode数据的方法，true的话使用qs库来解析，false的话使用querystring库去解决，qs库文档：https://www.npmjs.com/package/qs#readme
  "extended": true,

  // default = true
  // 是否开启压缩体解析
  "inflate": true,

  // default = '100kb'
  // 最大请求数据，传入数字默认单位是bytes，传入字符串要带上单位
  "limit": "100kb",

  // default = 1000
  // 控制url编码数据中最大参数数量，超过这个数量返回413
  "parameterLimit": 1000,

  // 接收数据的类型，默认是"application/x-www-form-urlencoded"
  "type": "application/x-www-form-urlencoded",

  // 验证数据，如果无效就可以提前抛出错误信息
  "verify": (req, res, buf, encoding) => {...}

}
```

- `bodyParser.text([options])`
  当默认数据类型为 `text/\*`时候会进入这个中间件处理，用的少，由于 json 数据更友好，能直接在数据库使用或是保存为 json 格式的文件，如果你更改下 `options.type = 'application/json'` 也可以处理 json 的数据。
  所以 `bodyParser.json()`相当于在此基础上进行封装优化，既然有更好用的，这个就不太用的上了，完全可以被取代。。options 多了一个解码方式的选择，`options.defaultCharset = 'utf-8'`，其他的都可以参考 `bodyParser.json()`，这里不做介绍。

- `bodyParser.raw([options])`
  处理默认数据为 `application/octet-stream` 时候的中间件，应用场景是 post 传入语音、短视频等媒体类型的数据，默认处理小于 100kb 的数据，以 buffer 的形式解析。参数参考前面的，都差不多。

#### multer 中间件

> express.json()、express.raw()、express.text()、express.urlencoded() 中间件，实际上都是基于 body-parser 中间件封装的；上述 4 种中间件对于处理大部分类型的 req.body 足够了，但却无法处理 multipart 类型的 req.body，body-parser 官网 也很明确的告诉我们 not handle multipart bodies，

multer 中间件来处理 multipart/form-data 类型数据。其主要用于上传文件，它是写在 busboy 之上非常高效。

```js
const express = require("express");
const multer = require("multer");
const router = express.Router();
const upload = multer();

router.post('/This_is_router_path', upload.['中间件方法'], function (req, res) {
  // req.body 获取文本域信息
  // req.file、req.files 获取上传的文件信息
})
module.exports = router;
```

##### 中间件方法

- `.none()` 仅文本域信息

```js
// .none() 不需要参数，使用该中间件的路由，只处理表单的文本域信息，保存在req.body
// 如果任何文件上传到这个模式，将发生"LIMIT_UNEXPECTED_FILE"错误
router.post("/upload/none", upload.none(), function (req, res) {
  let ret = {
    text_field: req.body,
    file_field: req.file,
  };
  res.send(ret);
});
```

- `.single()` 单文件上传

```js
// .single(fieldname) 接受一个以 fieldname 命名的文件，文件信息保存在 req.file
// 如果上传的文件多于1个，或者上传的文件名与fieldname不匹配，将发生"LIMIT_UNEXPECTED_FILE"错误
// 不指定filedname，则 .single() 等效于 .none()
router.post("/upload/single", upload.single("afile"), function (req, res) {
  let ret = {
    text_field: req.body,
    file_field: req.file, // req.file 是一个对象
  };
  res.send(ret);
});
```

- `.array()` 多文件上传

```js
// .array(fieldname[, maxCount]) 接受一个以 fieldname 命名的文件数组，maxCount 来限制上传的最大数量，文件信息保存在 req.files
// 如果上传的文件多于maxCount限定个数，或者上传了与fieldname不匹配的文件，将发生"LIMIT_UNEXPECTED_FILE"错误
// 不指定fieldname或maxCount=0，则与 .none()等效
router.post("/upload/array", upload.array("afile", 2), function (req, res) {
  let ret = {
    text_field: req.body,
    file_field: req.files, //req.files 是一个数组，每个元素是一个文件信息对象
  };
  res.send(ret);
});
```

- `.fields()` 多文件上传

```js
// .fields(fields) 接受指定 fields 的混合文件。
// fields 是对象数组，具有 name 和可选的 maxCount 属性，文件信息保存在 req.files
// 如果某name的文件上传的个数多于对应maxCount限定个数，或者上传了未指定name的文件，将发生"LIMIT_UNEXPECTED_FILE"错误
// .fields([]) 与 .none() 等效，注意如果[]也不传，则报路由找不到
const upFields = upload.fields([
  { name: "afile", maxCount: 2 },
  { name: "bfile", maxCount: 1 },
]);
router.post("/upload/fields", upFields, function (req, res) {
  let ret = {
    text_field: req.body,
    file_field: req.files, //req.files 是一个对象，键是文件名，值是文件数组
  };
  res.send(ret);
});
```

- `.any()` 多文件上传

```js
// .any() 不需要参数，使用该中间件的路由，可接受一切上传的文件，文件数组将保存在 req.files
// 未上传文件，则 req.files为[]，不会报错
router.post("/upload/any", upload.any(), function (req, res) {
  let ret = {
    text_field: req.body,
    file_field: req.files, //req.files 是一个数组，每个元素是一个文件信息对象
  };
  res.send(ret);
});
```

##### 上传控制

上边在做文件上传示例讲解时，实例化 multer()并没有传参。
实际上 multer(options) 可接受一个 options 对象，来控制上传的文件
options 对象及作用：

| Key             | Description                        |
| --------------- | ---------------------------------- |
| dest or storage | 在哪里存储文件                     |
| fileFilter      | 文件过滤器，控制哪些文件可以被接受 |
| limits          | 限制上传的数据                     |
| preservePath    | 保存包含文件名的完整文件路径       |

- 文件磁盘存储
  通过 dest 指定存储路径

```js
// 为了避免命名冲突，Multer 将为每个文件设置为一个随机文件名，并且是没有扩展名的
const upload = multer({ dest: __dirname + "/../../upload" });
```

通过 storage 指定存储路径

```js
// destination、filename 都是用来确定文件存储位置的函数
// destination 用来指定文件存储位置，指定的文件夹必须由用户自己创建，如果不找不到这个文件夹，则上传文件时会报路由找不到
// destination 如果不设置，则会使用系统默认的临时文件夹
// filename 用来给上传的文件重命名，如果不设置filename，则 Multer 将为每个文件设置为一个随机文件名，并且是没有扩展名的
// filename 指定的文件名要保证唯一性，不然上传同名的文件则会直接覆盖，不会有任何提示（示例中通过给文件名+时间戳来一定程度上避免重名情况）
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, __dirname + "/../../upload");
  },
  filename: function (req, file, cb) {
    cb(null, file.originalname + "-" + Date.now());
  },
});

const upload = multer({ storage: storage });
```

- 文件内存存储

```js
const storage = multer.memoryStorage();
const upload = multer({ storage: storage });
// 等效于 const upload = multer()，上边基本使用示例里就是用的不传任何参数的方式，可以看到不传任何参数时，默认使用了内存存储方式
// 注意：使用内存存储时，如果上传非常大的文件，或者非常多的小文件，可能会导致应用程序内存溢出
```

- 文件过滤
  Multer 通过 fileFilter 来设置一个函数来控制什么文件可以上传以及什么文件应该跳

```js
const filter = (req, file, cb) => {
  // 回调函数cb，通过boolean值来指示是否应接受该文件
  // 拒绝这个文件，使用`false`，cb(null, false)
  // 接受这个文件，使用`true`，cb(null, true)
  // 当然你也可以选择抛一个错误，cb(new Error('err_msg'))
  let ext = path.extname(file.originalname);
  req.refused_files = [];
  const refused_ext = [".png"];
  if (refused_ext.includes(ext)) {
    // return cb(new multer.MulterError("LIMIT_UNEXPECTED_FILE",file.fieldname));
    return cb(null, false, req.refused_files.push(file));
  }
  return cb(null, true);
};
const upload = multer({ fileFilter: filter });

// 这里 /upload/any 返回结果加一个refused_files，来测试上边的过滤配置是否生效
router.post("/upload/any", upload.any(), function (req, res) {
  let ret = {
    text_field: req.body,
    file_field: req.files,
    refused_files: req.refused_files,
  };
  res.send(ret);
});
```

- 大小限制
  使用 limits 对象，指定一些数据大小的限制

| Key           | Description                                              | Default    |
| ------------- | -------------------------------------------------------- | ---------- |
| fieldNameSize | field 名字最大长度                                       | 100 bytest |
| fieldSize     | field 值的最大长度                                       | 1MBt       |
| fields        | 非文件 field 的最大数量                                  | 无限 t     |
| fileSize      | 在 multipart 表单中，文件最大长度 (字节单位)             | 无限 t     |
| files         | 在 multipart 表单中，文件最大数量                        | 无限 t     |
| parts         | 在 multipart 表单中，part 传输的最大数量(fields + files) | 无限 t     |
| headerPairs   | 在 multipart 表单中，键值对最大组数量                    | 2000t      |

```js
// 数据大小限制
const limits = {
  fileSize: 1024 * 10, //限制上传文件大小为10kb
  files: 2, //限制文件上传个数最多2个
};
const upload = multer({ limits: limits });
```

- 完整路径
  Multer 通过 preservePath 来控制是否保存包含文件名的完整文件路径

```js
const upload = multer({ dest: __dirname + "/../../upload", preservePath: true });
```

##### 异常处理

当遇到一个错误，multer 将会把错误发送给 express
如果你想捕捉 multer 发出的错误，你可以自己调用中间件程序，可以使用 multer 对象下的 MulterError 类。

- 在路由的常规代码中，在使用 multer 中间件时进行错误处理，

```js
const upFields = upload.fields([
  { name: "afile", maxCount: 2 },
  { name: "bfile", maxCount: 1 },
]);
router.post("/upload/fields", function (req, res) {
  upFields(req, res, function (err) {
    if (err) {
      res.send(err);
    } else {
      let ret = { text_field: req.body, file_field: req.files };
      res.send(ret);
    }
  });
});

router.post("/upload/any", function (req, res) {
  upload.any()(req, res, function (err) {
    if (err) {
      res.send(err);
    } else {
      let ret = { text_field: req.body, file_field: req.files, refused_files: req.refused_files };
      res.send(ret);
    }
  });
});
```

- 在路由之前可以通过使用 app.use() 或 router.use() 对错误进行统一处理
  > 注意：router.use()错误处理函数 要放在所有 router.METHOD 之后；同样的，app.use()错误处理函数，也需要放到 app.use(require(“./routes/multer_demo”)) 路由导入函数后

```js
router.post("/upload/none", upload.none(), function (req, res) {
  let ret = { text_field: req.body, file_field: req.file };
  res.send(ret);
});

// 对 multer 错误及其他未知错误进行统一处理
router.use(function (err, req, res, next) {
  if (err instanceof multer.MulterError) {
    // A Multer error occurred when uploading.
    res.status(500).send(err);
  } else if (err) {
    // An unknown error occurred.
    res.status(500).send(err);
  }
  next();
});
```

- 主动抛 multer 错误
  在定义 fileFilter 文件过滤函数时，默认在用户上传了不允许的文件后直接过滤掉了，不会报错。假如想要在用户上传了不允许的文件类型时，抛出 multer 错误，那么就可以在定义 fileFilter 文件过滤函数时，通过 multer.MulterError 主动抛一个 LIMIT_UNEXPECTED_FILE 错误，代码如下：

```js
const filter = (req, file, cb) => {
  let ext = path.extname(file.originalname);
  req.refused_files = [];
  const refused_ext = [".png"];
  if (refused_ext.includes(ext)) {
    const mError = new multer.MulterError("LIMIT_UNEXPECTED_FILE", file.fieldname);
    // 默认的message内容为Unexpected field，这里我自定义一个message
    mError.message = "Not allowed file type " + ext;
    return cb(mError);
    // return cb(null,false,req.refused_files.push(file));
  }
  return cb(null, true);
};
```

#### 构建模块化路由

```js
const express = require("express");
//创建路由对象
const home = express.Router();
//将路由和请求路径进行匹配
app.use("/home", home);
//在home路由下继续创建路由
home.get("/index", () => {
  // 响应/home/index
  res.send("welcome");
});
```

参数路由

```js
//服务端
app.get("/find/:id", (req, res) => {
  console.log(req.params); //{id:124}
});

//客户端
localhost: 3000 / find / 124;
```

#### express 模板引擎

在原 art-template 模板引擎的基础上进行封装:`npm install art-template express-art-template`

```js
//当渲染后缀为art的模板时，使用express-art-template
app.engine("art", require("express-art-template"));
//设置模板存放目录
app.set("views", path.jon(__dirname, "views"));
//渲染模板时不写后缀，默认拼接art后缀
app.set("view engine", "art");
//渲染模板
app.get("/", (req, res) => {
  res.render("index", { mes: "模板数据" });
});
```

将变量设置到 app.locals 对象下，则所有的模板都可以获取到

```js
app.locals.users = [
  {
    //users是自定义的
    name: "xiaoming",
    age: 22,
  },
  {
    name: "xiaomei",
    age: 20,
  },
];
```

#### express-ws

```js
const express = require("express"); // 引入express插件包并生成一个实例app
const app = express();

// 引入express-ws的WebSocket功能，并混入app，相当于为 app实例添加 .ws 方法
const expressWs = require("express-ws")(app);
app.ws("/", (ws, req) => {
  // console.log('连接成功', ws)
  const clients = ws.getWss(); //获取所有链接的客户端

  ws.send("来自服务端推送的消息");
  ws.on("message", function (msg) {
    ws.send(`收到客户端的消息为：${msg}，再返回去`);
  });

  ws.on("close", function (e) {
    // console.log('连接关闭')
  });
});
```

### 第三方模块 mine

其中 mine.getType(路径);可根据路径返回请求的文件类型;可用于 res.writeHead(200,{'content-type':'text/css;charset=utf8'})

### 第三方模块 WS

webSocket 没有同源限制，客户端可以发送任意请求到服务端，只要目标服务器允许。

#### 服务端

```js
const WebSocket = require("ws");
const WebSocketServer = WebSocket.Server;

// 创建 websocket 服务器 监听在 3000 端口
const wss = new WebSocketServer({ port: 3000 });

// 服务器被客户端连接
wss.on("connection", (ws) => {
  // 通过 ws 对象，就可以获取到客户端发送过来的信息和主动推送信息给客户端

  var i = 0;
  var int = setInterval(function f() {
    ws.send(i++); // 每隔 1 秒给连接方报一次数
  }, 1000);
});
```

#### client.js

```js
const WebSocket = require("ws");
const ws = new WebSocket("ws://localhost:3000");

// 接受
ws.on("message", (message) => {
  console.log(message);

  // 当数字达到 10 时，断开连接
  if (message == 10) {
    ws.send("close");
    ws.close();
  }
});
```

#### web 浏览器

```js
// 浏览器提供 WebSocket 对象
const ws = new WebSocket("ws://127.0.0.1:2001");
ws.onopen = function () {
  ws.send("Hello Server!");
};
ws.onmessage = function (event) {
  //处理服务端发来的实际数据data
  if (typeof event.data === String) {
    console.log("Received data string");
  }

  if (event.data instanceof ArrayBuffer) {
    var buffer = event.data;
    console.log("Received arraybuffer");
  }
};
socket.onerror = function (event) {
  // handle error event
};
ws.onclose = function (event) {};
```

### 第三方模块 art-template 模板引擎

1. 下载：`npm install art-template`

2. 引入：`const template = require('art-template');`

3. 告诉模板引擎要拼接的数据和模板地址:`const html=template('模板数据',数据);`

实例

```js
//导入模块
const template = require('art-template');
//拼接
const html=template('./index.art',{
data:{
name: 'xioaming',
age: 29
}
});

//index.art
<div>
 <span>{{data.name}}</span>
 <span>{{data.age}}</span>
</div>
```

- 模板语法

  - 数据输出
    标准语法：`{{数据}}`
    原始语法：`<%=数据%>`

  - 原文输出：数据中带有 HTML 标签，默认情况下模板引擎不会解析标签,会转义后输出,若要解析则
    标准语法：`{{@数据}}`
    原始语法：`<%-数据%>`

  - 条件判断
    标准语法：`{{if 条件}}....{{else if 条件}}....{{/if}}`
    原始语法：`<%if (条件) {%>....<%} else if(条件) {%>....<%}  %>`

  - 循环
    标准语法：`{{each 数据}} {{$index}} {{$value}}  {{/each}}`
    原始语法：`<% for() {%> ....<%} %>`

- 子模板

将网站的公共区域（头部、页脚）抽离到单独文件中
标准语法：`{include '模板路径/footer.art'}`
原始语法：`<%include('模板路径')%>`

- 模板继承

将网站 HTML 骨架抽离到单独文件中，其他页面模板可以继承

```html
!<DOCTYPE HTML>
  <html lang="en">
    <head>
      <meta charset="utf-8" />
      <title>骨架模板</title>
      {{block 'head'}} {{/block}}
    </head>
    <body>
      {{block 'content'}} {{/block}}
      <!--预留位置-->
    </body>
  </html></DOCTYPE
>
```

模板继承

```js
 {{extend './test.art'}}
 {{block 'head'}} <link rel="stylesheet" href="test.css"> {{/block}}
```

- 模板配置

  1. 向模板中导入变量`template.default.imports.变量名=变量值(第三方模板的方法);` 在模板 test.art 中使用`{{dateFormat(time,'yyyy-mm-dd')}}`
  2. 设置模板根目录`template.defaults.root=模板目录`
  3. 设置模板默认后缀`template.defaults.extname='.art'`

### 第三方模块 router

功能：实现路由
使用步骤：获取路由对象；调用路由对象提供的方法创建路由；启用路由；

```js
const getRouter = require("router");
const router = getRounter();
router.get("/add", (req, res) => {
  res.end("hello world!");
});
server.on("request", (req, res) => {
  router(req, res);
});
```

### 第三方模块 serve-static

功能：静态资源访问服务

```js
const serverStatic = require("serve-static");
const serve = serverStatic("./pulic");
serve.on("request", () => {
  serve(req, res);
});
server.listen(3000);
```

### 第三方模块 Gulp

功能：项目上线，HTML、CSS、JS 文件压缩合并，语法转换（es6、less）

使用：

- 使用`npm install gulp`下载 gulp 库文件
- 在项目根目录下建立 gulpfile.js 文件
- 重构项目的文件夹结构 src 目录放置源代码文件，dist 目录放置构建后文件
- 在 gulpfile.js 文件中编写任务
- 在命令行工具中执行 gulp 任务

gulp 提供的方法：

- gulp.src():获取任务要处理的文件
- gulp.dest():输出文件
- gulp.task():建立 gulp 任务
- gulp.watch():监控文件的变化

gulp 插件：

- gulp-htmlminn: html 文件压缩
- gulp-csso: css 文件压缩
- gulp-babel: javascript 语法转化
- gulp-less: less 语法转化
- gulp-uglify: 压缩混淆 JavaScript
- gulp-file-include:公共文件包含
- browsersync:浏览器实时同步

## Node.js 异步编程

- 同步 api:当前 api 执行完成后，才能继续执行下一个 api;同步代码执行完后才开始执行异步代码，再执行回调函数

```js
//同步api可以从返回值中拿到api执行结果，而异步api不行

funciotn sum(x,y){
return x+y;
}

const re=sum(2,1);

//异步
function getMsg(){
setTimeout(function(){
return {msg:'hello'},1000);
}
const res=getMsg(); //undefined

```

- 异步 api:当前 api 的执行，不会阻塞后续代码的执行;异步 api 通过回调函数获取返回值

> 回调函数：自己定义函数让别人去调用

```js
function getData(callback) {
  callback("XiaoMing");
}

getData(function (n) {
  console.log("hello" + n);
});
```

- promise 解决回调地狱问题

```js
ler promise=new Promise((resolve,reject)=>{
setTimeout(()=>{
if(true){
resolve({name:'xiaoming'});
}else{
reject('error');
}
},2000);
}):
promise.then(result=>console.log(result);//{name:'xioaming'}).catch(error=>console.log(error);//error);
```

- 异步函数：终极方案，让异步代码写成同步代码形式，让代码不再有回调函数嵌套

```js
//在普通函数定义前加上async关键字
//异步函数默认返回的时promise对象
//在异步函数使用throw关键字进行错误抛出，之后的代码将不会执行
const fn = async () => {};
async function fn() {
  throw "error";

  return ok;
}

console.log(fn());
fn().then(function (data) {
  console.log(data);
});

//awaite关键字只能出现在异步函数中
//awaite promise对象； 暂停异步函数的执行，等待promise对象返回结果后在向下执行
async function run() {
  await p1();
  await p2();
  await p3();
}
```

返回 promise 对象

```js
//使用util模块的promisify方法
const fs=require('fs');
const promisify=require('util').promisify;
const readFile=pronisify(fs.readFile);
async function run(){

let r1=awaite readFile('./1.txt','utf8');
let r2=awaite readFile('./2.txt','utf8');
console.log(r1,r2);
}
run();
```

捕获错误:

`try catch`可以捕获异步函数以及其他同步代码在执行过程中发生的错误，但不能捕获其他类型 api 发生的错误。

```js
app.get("/", async (req, res, next) => {
  try {
    await User.fid({ name: "xiaoming" });
  } catch (er) {
    next(er);
  }
});
```

## 全局对象 global

在浏览器中全局对象时 window,在 node 中全局对象是 global

Node 中的全局对象有以下方法，可省略 global：`console.log()`,`setTimeout()`,clearEimeout()`,`setInternal()`,`clearInterval`

## 数据库

存储数据的仓库：mysql、MongoDB、Oracle

相关概念：

| 术语       | 说明                      |
| ---------- | ------------------------- |
| database   | 数据仓库,可建立多个数据库 |
| collection | 集合，一组数据的集合      |
| document   | 文档，一条具体的数据      |
| field      | 字段,文档中的属性名称     |

### 第三方包 Mongoose

使用 node.js 操作 MongoDB 数据库,下载`npm install mongoose`

- 数据库的连接

```js
const mongoose = require("mongoose");
mongoose
  .connect("mongodb://localhost/test")
  .then(() => console.log("数据库连接成功"))
  .catch((err) => console.log("fail!", err));
```

- 创建数据库

在 MongoDB 中不需要显示创建数据库，如果使用的数据库不存在，MongoDB 会自动创建。

#### MongoDB 增删查改操作

- 创建集合

```js
//对集合设定规则
const courseSchema = new mongoose.Schema({
  name: String,
  author: String,
  isPublished: Boolean,
});
//创建集合并合并规则
const Course = mongoose.model("Course", courseSchema); //返回构造函数
```

- 创建文档

即向集合中插入元素

第一种方法:

```js
//创建集合实例
const course=new Course({
name: 'tset',
author: 'xiaoming'
isPubulished: true
});
//保存数据
course.save();
```

第二种方法：

```js
Course.create({
name: 'tset',
author: 'xiaoming'
isPubulished: true
},
(err,doc)=>{
//错误对象
console.log(err);
//当前文档
console.log(doc);
}
);

//create返回的也是promise对象
Course.create({
name: 'tset',
author: 'xiaoming'
isPubulished: true
}).then(doc=>console.log(doc)).catch(err=>console.log(err));
```

- mongoose 验证

在创建集合规则时，可以设置当前字段的验证规则，验证失败则输入插入失败

```js
const postSchema=new mongoose.Schema({
 title:{
  type: String,  //字段类型
  required: true  //true时代表该字段必填,不能为空
  default: '默认title' //默认值
  validate:{  //自定义验证器
  validator: v=>{
   //返回布尔值，true代表验证成功
   //v要验证的值
   return v.length>4
  }
   masseage: 'error!!!!!';
  }
  },
 authod:{
  type: String,  //字段类型
  trim:true //自动去除字符串两边的空格
  required: [true,'错误信息'], //自定义错误信息
  minlength: 2,  //字符串最小长度
  maxlength: [10,'长度错误'],  //字符串最大长度

  },
 age:{
  type: Number,
  min: 2,  //数值的最小值
  max: 100 //数值的最大值
 },
 category:{
  type:String,
  enum:['html','css','js']  //枚举,输入的值必须在其中,否则报错
  /*
  enum:{
   values: ['html','css','js'],
   message: 'error message'
  }
  */
 }

});

const Post=mongoose.model('Post',postSchema);
post.create({}).then(re=>console.log(re)).catch(err=>console.log(err));
```

- 集合关联

  1. 使用 id 对集合进行关联
  2. 使用 populate 方法进行关联集合查询

```js
//用户集合
const User=mongoose.model('User',new mongoose.Schema({name:{type: String}}));
//文章集合
const Post=mongoose.model('Post',new mongoose.Schema({
 title:{type: String},
//使用id将文章集合和作者集合进行关联
 author:{type: mongoose.Schema.Types.ObjectId, ref: 'User'}
})):

//联合查询
Post.find().populate('author').then((err,result)=>console.log(result));
```

- 导入数据

shell 命令：`mongoimport -d 数据库名称 -c 集合名称 -file 要导入的数据文件（josn格式）`

- 查询文档

```js
//根据条件查询文档，若为空则查询所有文档
Course.find().then((result) => console.log(result)); //返回的是文档的集合数组

//实例
Course.find({ name: "xiaoming" }).then((result) => console.log(result));
User.find({ age: { $gt: 20, $lt: 50 } }).then((result) => console.log(result)); //查询大于20小于50
User.find({ hobbies: { $in: ["运动"] } }).then((result) => console.log(result)); //查询包含运动的
```

```js
//只查询一条数据
Course.findOne({ name: "xiaoming" }).then((result) => console.log(result));
```

```js
//选择要查询的字段
User.find()
  .select("name age -_id")
  .then((result) => console.log(result))
  .catch((err) => console.log(err)); //只返回查询的字段，不查询的字段前加-
```

```js
//将查询出来的数据进行升序排序(从小到大）
User.find()
  .sort("age")
  .then((result) => console.log(result));

//将查询出来的数据进行降序排序(从大到小）
User.find()
  .sort("-age")
  .then((result) => console.log(result));
```

```js
//skip跳过多条数据，limit限制查询数量
User.find()
  .skip(2)
  .limit(2)
  .then((result) => console.log(result)); //分页查询时常用
```

- 删除文档

```js
//删除第一个文档
Course.findOndeAndDelete({}).then((result) => console.log(result)); //返回删除的文档
```

```js
//删除多个文档
User.deleteMany({}).then((result) => console.log(result));
```

- 更新文档

```js
//更新单个
User.updateOne({ 查询条件 }, { 要修改的值 }).then((re) => console.log(re));
```

```js
//更新多个
User.updateMany({ 查询条件 }, { 要修改的值 }).then((re) => console.log(re));
```

#### mysql

##### 综合案例

```js
//301代表重定向
//location 跳转地址
req.on("end", () => {
  res.writeHead(301, {
    Location: "/list",
  });
  res.end();
});
```

`npm i nodemon -g`
`nodemon test.js`
