---
title:  NodeJs
date: 2020-11-12 20:11:33
catergories:
- study
---

# NodeJs

## Node.js模块化开发

Node.js规定一个Javascript文件就是一个模块，模块内部定义的变量和函数默认情况下外部无法得到。
模块内部可以使用exports对象进行成员导出，使用require方法导入其他模块。

### 模块成员导出

```
//a.js
//在模块中定义变量
let version=1.0;
//模块中定义方法
const sayHi=name=>`hello！${name}`	//es6新语法
//向模块外部导出数据
exports.version=version;
exports.sayHi=sayHi;
```

### 模块成员的导入

```
//b.js
//在b.js模块中导入模块a
let a=require('./b.js');
//输出b模块中的version变量
console.log(a.version);
//调用b模块中的sayHi方法
console.log(a.sayHi('XiaoMing'));
```
模块成员导出的另一种方法
```
module.exports.version=version;
module.exports.sayHi=sayHi;
```
exports是module.exports的别名，导出对象最终以module.exports为准；


## package.json文件的作用

项目描述文件，记录了当前项目信息。如：项目版本、作者、依赖的模块等等；使用`npm init -y`生成。其作用为（一）锁定包的版本，确保再次下载时不会因为包版本不同而产生问题。（二）加快下载速度，其记录着依赖的下载地址。


- 开发依赖

在项目开发阶段所需要的依赖，线上运营阶段不需要依赖的第三方包,称为开发依赖；可使用`npm install 包名 --save-dev`将包添加到package.json文件的devDependencies字段中
```
{
	"devDependencies":{
		"gulp":"^3.9.1"
	}
}
```


## Node.js中模块的加载机制

模块查找规则

- 当模块有路径没后缀时

```
require('./find.js);
require('./find');
```

   1. require方法根据路径查找模块
   2. 如果模块后缀省略，则先找同名JS文件再找同名JS文件夹
   3. 如果找到同名文件夹，则继续寻找其中的index.js文件
   4. 如果没有index.js文件，就会去当前文件夹中的package.js文件中查找main选项中的入口文件
   5. 如果入口文件不存在或没有指定入口文件，则报错：模块未找到

- 当模块即没路径也没后缀时

```
require("find");
```
   1. Node.js会假设它是系统模块，会去node_modules文件中寻找
   2. 先查看是否有同名js文件
   3. 在看有没有同名文件夹
   4. 若有，则继续寻找其中的index.js文件
   5. 如没有，则查看该文件夹中的package.json中的main选项确定模块的入口文件
   6. 若都没有，则报错

## 系统模块:Node运行环境提供的API

### fs文件操作

`const fs=require('fs');`

读取文件内容:`fs.readFile('文件路径/文件名'[,'文件编码'],callback);`(括号内为可选项)

实例：

```
//读取上一级css目录下的base.css
fs.readFile('../css/base.css','utf-8',(err,doc)=>{
//如果读取错误参数err的值为错误对象，否则err的值为null
//doc参数为文件内容
if(err==null){
//控制台中输出文件内容
console.log(doc);
}
});

```

写入文件内容：`fs.writeFile('文件路径/文件名称','数据',callback);

实例：

```
const conetent='<h3>!!!!!!!!!!</h3>';
fs.writeFile('../index.html',content,err=>{
if(err!=null){
console.log(err);
return;
}
console.log('文件写入成功');
});
```

### 路径拼接

`path.join('路径','路径',..);`

```
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

## 第三方模块

npm(node package manager):node的第三方模块管理工具

- 下载：`npm install 模块名`
- 卸载：`npm uninstall package 模块名`

全局安装和本地安装：一般命令行工具全局安装，库文件本地安装

### 第三方模块mine

其中mine.getType(路径);可根据路径返回请求的文件类型;可用于res.writeHead(200,{'content-type':'text/css;charset=utf8'})


### 第三方模块Gulp

功能：项目上线，HTML、CSS、JS文件压缩合并，语法转换（es6、less）

使用：
- 使用`npm install gulp`下载gulp库文件
- 在项目根目录下建立gulpfile.js文件
- 重构项目的文件夹结构src目录放置源代码文件，dist目录放置构建后文件
- 在gulpfile.js文件中编写任务
- 在命令行工具中执行gulp任务

gulp提供的方法：
- gulp.src():获取任务要处理的文件
- gulp.dest():输出文件
- gulp.task():建立gulp任务
- gulp.watch():监控文件的变化

gulp插件：
- gulp-htmlminn: html文件压缩
- gulp-csso:  css文件压缩
- gulp-babel: javascript语法转化
- gulp-less: less 语法转化
- gulp-uglify: 压缩混淆JavaScript
- gulp-file-include:公共文件包含
- browsersync:浏览器实时同步

## HTTP协议

超文本传输协议。规定了客户端（浏览器）与服务器（网站服务器）之间请求和应答的标准

- 静态资源：服务器不需要处理，可以直接响应给客户端的资源，如：css，JavaScript，image文件
- 动态资源：相同的请求地址，不同的响应资源

### 报文

在HTTP请求和响应的过程中传递的数据块叫做报文

- 请求报文

   1. 请求方式：GET、POST
   2. 请求地址

```
app.on('request',(req,res)=>{
req.headers	//请求报文
req.url		//请求地址
req.method	//请求方法
});
```

   3. 请求参数



- 响应报文

   1. HTTP状态码： 200(请求成功)、400(客户端请求语法有误)、404(请求的资源未找到)、500(服务器错误)、
   2. 内容类型：
      - text/html
      - text/css
      - application/javascript 
      - image/jpeg
      - application/json

### 路由

指客户端请求地址,如：`http://localhost:8080/index`与服务端程序代码的对应关系，即请求什么响应什么；

```
app.on('request',(req,res)=>{
//获取客户端的请求路径
let {pathname}=url.parse(req.url);
//或写成
//let pathname=url.parse(req.url).pathname;
if(pathname=='/'||pathname=='/index'){
res.end('欢迎来到首页');
}
else
{
res.end('页面不存在');
}
});
```


## 创建web服务器


```
//引用系统模块
const http =require('http');
//创建web服务器
const app = http.createServer();
//当客户端发送请求的时候
app.on('request',(req,res)=>{
//响应

res.end('hello');

//返回状态码和返回资源类型,若无，低级浏览器可能出问题

res.writeHead(400,{'content-type':'text/html;charset=utf8'});

});

//监听端口
app.listen(3000);
console.log("Server is running....");
```


完善：


```
//引用系统模块
const http =require('http');
const url=require('url');	//处理url地址模块
const querystring=require('querystring');	//处理参数模块

//创建web服务器
const app = http.createServer();


//当客户端发送请求的时候
app.on('request',(req,res)=>{
//post参数是通过事件的方式接受的
//data当请求参数传递的时候发出data事件
//end当参数传递完成的时候时发出end事件
//获取POST参数需要使用data事件和end事件；
let postParams='';
req.on('data',(params)=>{
postParams+=params;	//接收的数据不是一次性发完的
});
req.on('end',()=>{
console.log(postParams);
console.log(querystring.parse(postParams));	//将参数转换为对象格式
});

//响应
res.end('hello');

//返回状态码和相关信息

res.writeHead(400,{'content-type':'text/html;charset=utf8'});

//调用url模块方法parse();第一个参数为要处理的地址，第二个参数为是否处理为对象格式，true为是；
console.log(url.parse(res.url,true));

});

//监听端口
app.listen(3000);
console.log("Server is running....");
```


## Node.js异步编程

#### 同步api:当前api执行完成后，才能继续执行下一个api;同步代码执行完后才开始执行异步代码，再执行回调函数

```
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
const res=getMsg();	//undefined

```

#### 异步api:当前api的执行，不会阻塞后续代码的执行;异步api通过回调函数获取返回值；

- 回调函数：自己定义函数让别人去调用

```
function getData(callback){
callback('XiaoMing');
}

getData(function(n){ console.log('hello'+n);});
```

- promise解决回调地狱问题

```
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

```
//在普通函数定义前加上async关键字
//异步函数默认返回的时promise对象
//在异步函数使用throw关键字进行错误抛出，之后的代码将不会执行
const fn=async()=>{};
async function fn(){
throw 'error';

return ok;
}

console.log(fn());
fn().then(function(data){
console.log(data);
})

//awaite关键字只能出现在异步函数中
//awaite promise对象； 暂停异步函数的执行，等待promise对象返回结果后在向下执行
async function run(){
await p1();
await p2();
await p3();
}
```


返回promise对象

```
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



## 全局对象global

在浏览器中全局对象时window,在node中全局对象是global

Node中的全局对象有以下方法，可省略global：`console.log()`,`setTimeout()`,clearEimeout()`,`setInternal()`,`clearInterval`


## 数据库

存储数据的仓库：mysql、MongoDB、Oracle


相关概念：

|术语|说明|
|-|-|
|database|数据仓库,可建立多个数据库|
|collection|集合，一组数据的集合|
|document|文档，一条具体的数据|
|field|字段,文档中的属性名称|


### 第三方包Mongoose

使用node.js操作MongoDB数据库,下载`npm install mongoose`

- 数据库的连接

```
const mongoose=require('mongoose');
mongoose.connect('mongodb://localhost/test')
.then(()=>console.log('数据库连接成功'))
.catch(err=>console.log('fail!',err));
```

- 创建数据库

在MongoDB中不需要显示创建数据库，如果使用的数据库不存在，MongoDB会自动创建。


#### MongoDB增删查改操作

- 创建集合

```
//对集合设定规则
const courseSchema= new mongoose.Schema({
name: String,
author: String,
isPublished: Boolean
});
//创建集合并合并规则
const Course=mongoose.model('Course',courseSchema);//返回构造函数
```


- 创建文档

即向集合中插入元素

第一种方法:
```
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
```
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

- 导入数据

shell 命令：`mongoimport -d 数据库名称 -c 集合名称 -file 要导入的数据文件（josn格式）`


- 查询文档

```
//根据条件查询文档，若为空则查询所有文档
Course.find().then(result=>console.log(result));	//返回的是文档的集合数组

//实例
Course.find({name:'xiaoming'}).then(result=>console.log(result));
User.find({age:{$gt:20,$lt:50}}).then(result=>console.log(result));	//查询大于20小于50
User.find({hobbies:{$in:['运动']}}).then(result=>console.log(result));	//查询包含运动的
```

```
//只查询一条数据
Course.findOne({name:'xiaoming'}).then(result=>console.log(result));	
```

```
//选择要查询的字段
User.find().select('name age -_id').then(result=>console.log(result)).catch(err=>console.log(err));	//只返回查询的字段，不查询的字段前加-
```


```
//将查询出来的数据进行升序排序(从小到大）
User.find().sort('age').then(result=>console.log(result));	

//将查询出来的数据进行降序排序(从大到小）
User.find().sort('-age').then(result=>console.log(result));	
```

```
//skip跳过多条数据，limit限制查询数量
User.find().skip(2).limit(2).then(result=>console.log(result));		//分页查询时常用
```
