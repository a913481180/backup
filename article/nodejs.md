---
title:  NodeJs
date: 2020-11-12 20:11:33
catergories:
- study
---

# NodeJs


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

## HTTP协议

超文本传输协议。规定了客户端（浏览器）与服务器（网站服务器）之间请求和应答的标准

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


- 响应报文

   1. HTTP状态码： 200(请求成功)、400(客户端请求语法有误)、404(请求的资源未找到)、500(服务器错误)、
   2. 内容类型：
      - text/html
      - text/css
      - application/javascript 
      - image/jpeg
      - application/json


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
});

//监听端口
app.listen(3000);
console.log("Server is running....");
```

