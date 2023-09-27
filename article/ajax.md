---
title: AJax
date: 2021-02-12 20:10:33
categories:
- web
---

AJAX全称为“Asynchronous JavaScript and XML”（异步JavaScript和 XML），是一种创建交互式网页应用的网页开发技术。通过在后台与服务器进行少量数据交换，Ajax可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。而传统的网页(不使用 Ajax)如果需要更新内容，必需重载整个网页面。
Ajax的工作原理相当于在用户和服务器之间加了一个中间层(AJAX引擎)，使用户操作与服务器响应异步化

>`XMLHttpRequest`IE8以下不兼容；IE8以下声明ajax方法为`ActiveXObject("Microsoft.XMLHTTP");`

## AJAX请求的五个步骤

1. 创建ajax(XMLHttpRequest)对象

    ```js
    var xhr=null
    if(window.XMLHttpRequest){
        xhr=new XMLHttpRequest();
    }else{
        xhr=new ActiveXObject("Microsoft.XMLHTTP");
    }
    ```

2. `onreadystatechange`事件,设置回调函数,等待数据响应

    发送请求将会收到响应，XHR对象以下属性将会被填充上数据

    |readyState属性|HTTP请求状态|
    |-|-|
    |0|（请求还未初始化）还没调用open（）方法|
    |1|（已建立服务器链接）open() 方法已经被调用。未调用send()方法。|
    |2|（请求已接收）send() 方法已经被调用，并且头部和状态已经可获得|
    |3|（正在处理请求）下载中；responseText 属性已经包含部分数据。|
    |4|（完成）下载操作已完成。|

    |status|状态码英文名称|服务器（请求资源）的状态|
    |-|-|-|
    |200|OK|请求成功|
    |204|No Content|无内容。服务器成功处理，但未返回内容。|
    |206|Partial Content|是对资源某一部分的请求，服务器成功处理了部分GET请求，响应报文中包含由Content-Range指定范围的实体内容。|
    |301|Moved Permanently|永久性重定向。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替|
    |302|Found|临时性重定向。与301类似。但资源只是临时被移动。客户端应继续使用原有URI|
    |303|See Other|查看其它地址。与302类似。使用GET请求查看|
    |304|Not Modified|未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。|
    |307|Temporary Redirect |临时重定向。与302类似。使用GET请求重定向，会按照浏览器标准，不会从POST变成GET。|
    |400|Bad Request|客户端请求报文中存在语法错误，服务器无法理解|
    |401|Unauthorized|请求要求用户的身份认证，通过HTTP认证（BASIC认证，DIGEST认证）的认证信息，若之前已进行过一次请求，则表示用户认证失败|
    |402|Payment Required|保留，将来使用|
    |403|Forbidden|服务器理解请求客户端的请求，但是拒绝执行此请求|
    |404|Not Found|服务器无法根据客户端的请求找到资源|
    |500|Internal Server Error|服务器内部错误，无法完成请求|
    |501|Not Implemented|服务器不支持请求的功能，无法完成请求|
    |502|Bad Gateway|错误网关|
    |503|Service Unavailable|由于超载或系统维护，服务器暂时的无法处理客户端的请求|
    |504|Gateway Time-out|网关超时|

    `responseText`:返回以文本形式存放的内容;
    `responseXML`:返回XML形式的内容,如果响应的内容类型是"text/xml"或"application/xml"，那就是包含响应数据的 XML DOM 文档。
    `statusText`：响应的 HTTP 状态描述

    ```js
    xhr.onreadystatechange=function(){
        if(xhr.readyState==4){
            if(xhr.status==200){
            console.log(xhr.responseText);
            }else{
                alert("error:"+xhr.status);
            }
        }
    }
    ```

3. 调用open方法与服务器建立链接

    ```js
    /*
    第一个参数： 请求方式 
    第二个：url
    第三个：是否异步，true异步，false同步
    */
    xhr.open("get","./1.txt",true);
    //或
    xhr.open("get","./get.php?name=yyyy&age=22&password=342",true);
    //或
    xhr.open("post","/test.php",true);
    ```

4. 调用send方法,向服务器发送数据

    ```js
    //get方法,不需要传递参数
    xhr.send();

    //post方法，将数据放在send()里提交
    xhr.setRequestHeader('content-type','application/x-www-form-urlencoded'); //发送表单数据
    xhr.setRequestHeader('Content-type', 'application/json; charset=utf-8')//发送json格式数据
    xhr.setRequestHeader('Content-type', 'text/plain;  charset=utf-8')//发送纯文本,不指定Content-type时直接默认值
    xhr.setRequestHeader('Content-type', 'text/html; charset=utf-8')//发送html文本
    //设置请求头
    xhr.send("name=yyy&age=33&password=124564"); //无需编码
    //post没有缓存问题
    ```

### 传输格式

- xml数据
优点：种类丰富，传输量非常大
缺点：解析麻烦，不适合轻量数据

- json数据

优点：轻量级数据，解析轻松
缺点：种类少，传输数据量少

`JSON.parse();`:将JSON字符串转为一个对象。
你的字符串必须符合JSON格式，即键值都必须使用双引号包裹：

```js
let a = '["1","2"]';
let b = "['1','2']";
console.log(JSON.parse(a));// Array [1,2]
console.log(JSON.parse(b));// 报错
```

`JSON.stringify();`:将 JavaScript 对象转换为 JSON 字符串

### ajax 只能下载同源的数据，跨源的数据禁止下载
>
>如果要请求的域和当前域是不同域，就叫跨域

- 同源策略

   1. 同协议
   2. 同域名/IP
   3. 同端口号

同源策略禁止跨源请求

- 跨源方法：
   1. 修改ajax同源协议（不建议）
   2. 委托php文件进行跨源
   3. JSONP

### JSONP跨域的使用流程

   1. 先去声明一个函数，这个函数有一个形参，这个形参会拿到我们想要下载的数据，
   2. 在需要下载数据的时候，动态创建script标签，将标签src属性设置成下载数据的链接
   3. 当script插入到页面的时候，就会调用号已经封装好的函数，将数据传过来

index.html

```js
<script>
function download(data){
console.log(data);
}
</script>
<script src="test.js"> //浏览器一运行就会显示</script>
<script>
//动态生成script标签
window.onload=function(){
var Obtn=document.getElementById('btn1');
Obtn.onclick=function(){
 var oScript=document.createElement("script"); //创建script标签
 oScript.src='test.js';
 document.body.appendChild(oScript);  //插入到页面
}
}
</script>
```

 test.js

```js
download("hello!!!!!!!!!");
```

实例：

```html
<script>
function download(data){
console.log(data);
 var oInfo=documenet.getElementById("oinfo");
 var oTi=..;
 oInfo.innerHTML=`${data.city}`
  
 var str='';
 for (var i=0;i<arr.length;i++){
 str+=`
  <tr>
   <td>${arr[i].data}</td>
  </tr>
   `
  }

 oTi.innerHTML=str;
}
</script>
<script>
//动态生成script标签
window.onload=function(){
var Obtn=document.getElementById('btn1');
Obtn.onclick=function(){
 var oScript=document.createElement("script"); //创建script标签
 oScript.src='https:.....&callback=download';
 document.body.appendChild(oScript);  //插入到页面
}
}
</script>
```

### 表单提交

form表单点击提交数据后需跳转页面，ajax为异步进行的数据传输

- action:点击submit后跳转的url
- method:表单提交方式get/post
  - get(默认)
直接将数据拼接在url后面进行提交;用`？`进行拼接,多个数据之间用`&`进行连接
    - 优点：简单
    - 缺点：不安全，最大2kb，无法实现上传大文件

```html
<form action="get.php" method="get">
<input type="text" name="username" placeholder="name" />
<input type="text" name="age" placeholder="age" />
<input type="text" name="password" placeholder="" />
<input type"submit"/>
</form>
```

- post
通过浏览器内部进行提交
  - 优点：安全，上传大小无上限
  - 缺点：比get复杂

`enctype`提交数据的格式，默认`application/x-www-form-urlencoded`

```html
<form action="post.php" method="post"  enctype="application/x-www-from-urlencoded">
<input type="text" name="username" placeholder="name" />
<input type="text" name="age" placeholder="age" />
<input type="text" name="password" placeholder="" />
<input type"submit" value="post按钮"/>
</form>
```

php

```php
<?php
header('content-type:text/html;charset="UTF-8"');
//$_GET 全局关联数组 ；存放着get提交的所有数据
//$_POST全局数组，储存着post发送来的数据
$username=$_GET['username'];
$age=$_GET['age'];
$password=$_GET['password'];

echo ("{$username},${age},{$password});
?>
```

### 数据库操作

```php
<?php
header("Content-type:text/html;charset=utf-8");
//连接数据库
/*
第一个参数：连接数据库的ip/域名
第二个参数：用户名
第三个参数：密码
*/
$link=mysql_connect("localhost","root","123456");
if($link){
echo"连接失败";
exit;
}

//设置字符集
mysql_set_charset("utf8");
//选择数据库
mysql_select_db("jjjj");
//准备sql语句
$sql="SELECT * FROM students";
//发送sql语句
$res=mysql_query($sql);
//处理结果,全部获取
$arr=array();
while($row=mysql_fetch_assoc($res)){
array_push($arr,$row);
var_dump($row);
}
echo json_encode($arr);
//关闭数据库
mysql_close($link);

?>
```

---

## post/put/patch请求传参格式有 formData 形式 、query 形式 、JSON形式三种

## axios设置token到请求头

加一个http request拦截器；通过window.localStorage.getItem("accessToken") 来获取token的value；通过config.headers.accessToken = token;将token放到请求头发送给服务器，放在请求头中

## form表单提交是单向的

只能给服务器发送数据，但是无法获取服务器返回的数据，也就是无法读取HTTP应答包
---

同源策略
跨域：浏览器允许向服务器发送跨域请求，从而克服Ajax只能同源使用的限制。

同源策略：如果两个页面的协议，域名,端口都相同，则两个页面具有相同的源。
同源策略是浏览器的一个安全功能，不同源的客户端脚本在没有明确授权的情况下，不能读写对方资源。这是一个用于隔离潜在恶意文件的重要安全机制。

不受同源策略限制的

页面中的链接，重定向以及表单提交是不会受到同源策略限制的。

跨域资源的引入是可以的。但是js不能读写加载的内容。如嵌入到页面中的`<script src="..."></script>，<img>，<link>，<iframe>`等。

受到限制的

Cookie、LocalStorage 和 IndexDB 无法读取
DOM和JS对象无法获得
AJAX 请求不能发送
跨域解决方案
一、JSONP跨域
jsonp的核心原理就是：目标页面回调本地页面的方法,并带入参数
服务器端实现 JSONP 接口的步骤
服务器端获取客户端发送过来的query参数，其中参数有回调函数的名字
得到的数据，拼接出一个函数调用的字符串
把上一步拼接得到的字符串，响应给客户端的 `<script>` 标签进行解析执行
jsonp的缺点：只能发送get一种请求。
1、原生JS实现
通过script标签src属性，发送带有callback参数的GET请求，服务端将接口返回数据拼凑到callback函数中，返回给浏览器，浏览器解析执行，从而前端拿到callback函数返回的数据。

```html
<script>
    function getData(data){
        console.log(data)
    }
</script>
<script src="http://127.0.0.1:3000/web?cb=getData"></script>
```

后端nodejs代码
主要用来模拟服务器
携带参数必须是字符串

```js
const express=require('express')
const router=express.Router()
router.get('/web',(req,res)=>{
    let {cb}=req.query
    console.log(req.query)
    var data = {
        name: 'xtt',
        age: 18,
        gender:'女孩子'
    }
    // 携带参数必须是字符串
    res.send(`${cb}(${JSON.stringify(data)})`)
    router.get('/que',(req,res)=>{
        res.send(`${req.query.cb}('dd')`)
    })
})
module.exports=router
```

2、jquery Ajax实现
以jquery来发起jsonp请求

```html
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.10.0/jquery.js"></script>
<script>
    let url = 'http://127.0.0.1:3000/que?cb=getData'
    $.ajax({
        method: 'GET',
        url,
        dataType: 'jsonp',
        success: (res) => {
            console.log(res)
        }
    })
</script>
```

3、Vue axios实现

```js
handleCallback({"success": true, "user": "admin"})
this.$http = axios;
this.$http.jsonp('<http://127.0.0.1:3000/que?cb=getData>', {
    params: {},
    jsonp: 'handleCallback'
}).then((res) => {
    console.log(res);
})
```

二、跨域资源共享（CORS）
CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。
它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。
CORS需要浏览器和服务器同时支持。
目前，所有主流浏览器都支持该功能，IE10以下不支持。
浏览器将CORS跨域请求分为：简单请求、非简单请求。
简单请求与非简单请求
简单请求
浏览器在发送跨域请求的时候，会先判断下是简单请求还是非简单请求，如果是简单请求，就先执行服务端程序，然后浏览器才会判断是否跨域。
同时满足以下的两个条件，就属于简单请求。浏览器对这两种的处理，是不一样的。

请求方式：get/post/head其中一种
请求头设置：

```txt
Accept
Accept-Language
Content-Type：application/x-www-form-urlencoded、multipart/form-data、text/plain（ 只限于三个值中的一个）
```

详细描述
对于简单请求，浏览器直接发出CORS请求。具体来说，就是在头信息之中，增加一个Origin字段。
举例：

发起请求

自动在头信息之中，添加一个Origin字段。

```txt
GET /cors HTTP/1.1
Origin: <http://127.0.0.1:8080>
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
Origin：本次请求来自哪个域（协议 + 域名 + 端口）。服务器根据这个值，决定是否同意这次请求。
```

服务器判断此次请求Origin源

不在许可范围内：服务器会返回一个正常的 HTTP 回应。
浏览器发现，这个回应的头信息没有包含Access-Control-Allow-Origin字段（详见下文），就知道出错了，从而抛出一个错误，被请求的异常回调函数捕获。
注意，这种错误无法通过状态码识别，因为 HTTP 回应的状态码有可能是200。
在许可范围内：服务器返回的响应，会多出几个头信息字段。
有三个与 CORS 请求相关的字段，都以Access-Control-开头。

```txt
Access-Control-Allow-Origin: <http://api.bob.com>
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8
```

Access-Control解释

Access-Control-Allow-Origin：必须的

它的值要么是请求时Origin字段的值，要么是一个*，表示接受任意域名的请求。

Access-Control-Allow-Credentials：可选

布尔值，表示是否允许发送 Cookie。默认情况下，Cookie 不包括在 CORS 请求之中（为了降低 CSRF 攻击的风险。）。设为true，即表示服务器明确许可，浏览器可以把 Cookie 包含在请求中，一起发给服务器。这个值也只能设为true，如果服务器不要浏览器发送 Cookie，不发送该字段即可。

Access-Control-Expose-Headers：可选

CORS 请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个服务器返回的基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。上面的例子指定，getResponseHeader('FooBar')可以返回FooBar字段的值。

非简单请求
对服务器提出特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。

预检请求

非简单请求的 CORS 请求，会在正式通信之前，增加一次 HTTP 查询请求，称为“预检”请求（preflight）。
浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些 HTTP 方法和头信息字段。
只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。
这是为了防止这些新增的请求，对传统的没有 CORS 支持的服务器形成压力，给服务器一个提前拒绝的机会，这样可以防止服务器收到大量DELETE和PUT请求，这些传统的表单不可能跨域发出的请求
举例

自动发出一个“预检”请求，要求服务器确认可以这样请求。下面是这个“预检”请求的 HTTP 头信息：

```txt
OPTIONS /cors HTTP/1.1
Origin: <http://api.bob.com>
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

两个特殊字段:

Access-Control-Request-Method必须的
用来列出浏览器的 CORS 请求会用到哪些 HTTP 方法，上例是PUT。

Access-Control-Request-Headers
该字段是一个逗号分隔的字符串，指定浏览器 CORS 请求会额外发送的头信息字段。

服务器收到“预检”请求以后，检查了Origin、Access-Control-Request-Method和Access-Control-Request-Headers字段以后，确认允许跨源请求，就可以做出回应

预检请求的回应：
服务器收到"预检"请求以后，检查了Origin、Access-Control-Request-Method和Access-Control-Request-Headers字段以后，确认允许跨源请求，就可以做出回应。

HTTP回应中，除了关键的是Access-Control-Allow-Origin字段，其他CORS相关字段如下：

Access-Control-Allow-Methods：必选
它的值是逗号分隔的一个字符串，表明服务器支持的所有跨域请求的方法。注意，返回的是所有支持的方法，而不单是浏览器请求的那个方法。这是为了避免多次"预检"请求。

Access-Control-Allow-Headers
如果浏览器请求包括Access-Control-Request-Headers字段，则Access-Control-Allow-Headers字段是必需的。它也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段，不限于浏览器在"预检"中请求的字段。

Access-Control-Allow-Credentials：可选
该字段与简单请求时的含义相同。

Access-Control-Max-Age：可选
用来指定本次预检请求的有效期，单位为秒。

CORS跨域
1)前端设置

```js
let xhr;
try {
    xhr=new XMLHttpRequest();
} catch (error) {
     xhr=new ActiveXObject('Microsoft.XMLHTTP');
}
xhr.open('post','<http://localhost:3000/login',true>);
xhr.setRequestHeader('content-type','application/x-www-form-urlencoded');
xhr.send('name=111&age=12');
xhr.onreadystatechange=function(){
    if(xhr.readyState==4){
        let reg=/^2\d{2}/
        if(reg.test(xhr.status)){
            console.log(JSON.parse(xhr.response))
        }
    }
}
```

nodejs代码
在Express中通过第3方中间件来完成cors跨域解决
使用步骤分为如下 3 步：

运行` npm install cors `安装中间件
使用 `const cors = require('cors')`导入中间件
在路由之前调用 `app.use(cors())` 配置中间件

```js
const express=require('express')
const cors=require('cors')
const app=express()
app.listen(3000)
const allowHosts=[
    'http://localhost:5000',
    'http://localhost:2000'
]
app.use(cors())
app.use((req,res,next)=>{
    let hst =req.header.origin
    if(allowHosts.includes(hst)){
        next()
    }else{
        return res.send({
            code:404,
            msg:'地址不对'
        })
    }
})
app.get('/login',(req,res)=>{
    res.send('登陆')
})
```

三、Nginx 反向代理解决跨域问题
正向代理和反向代理
提到代理，肯定要说一下这两个的区别。

举个正向代理的例子
我打球累了走不动了，找看球的小朋友帮我去旁边的商店买瓶水。商店老板是不知道到底是谁需要喝水的，隐藏了客户端。当然，小朋友可以告诉老板就是那个打球像蔡徐坤的人要喝水。还有，VPN 就是正向代理。

反向代理的例子
我打球累了，找看球的小朋友要瓶水喝（当然我肯定会给钱的：D）。我不需要知道小朋友的水是从旁边的商店还是两公里外的超市买的。隐藏了服务端。还有，我们连好了 VPN 访问谷歌的时候，浏览的那些页面，我们是不会知道具体是哪台服务器的资源。

nginx配置解决iconfont跨域
浏览器跨域访问js、css、img等常规静态资源被同源策略许可，但iconfont字体文件(eot|otf|ttf|woff|svg)例外，此时可在nginx的静态资源服务器中加入以下配置。

```conf
location / {
  add_header Access-Control-Allow-Origin *;
}
```

nginx反向代理接口跨域
跨域问题：同源策略仅是针对浏览器的安全策略。服务器端调用HTTP接口只是使用HTTP协议，不需要同源策略，也就不存在跨域问题。

实现思路：通过Nginx配置一个代理服务器域名与domain1相同，端口不同）做跳板机，反向代理访问domain2接口，并且可以顺便修改cookie中domain信息，方便当前域cookie写入，实现跨域访问。

nginx具体配置

# proxy服务器

```conf
server {
    listen       81;
    server_name  <www.domain1.com>;

    location / {
        proxy_pass   http://www.yp2.com:8080;  #反向代理
        proxy_cookie_domain www.yp2.com www.yp1.com; #修改cookie里域名
        index  index.html index.htm;

        # 当用webpack-dev-server等中间件代理接口访问nignx时，此时无浏览器参与，故没有同源限制，下面的跨域配置可不启用
        add_header Access-Control-Allow-Origin http://www.yp1.com;  #当前端只跨域不带cookie时，可为*
        add_header Access-Control-Allow-Credentials true;
    }
}
```

四、nodejs中间件代理跨域
node中间件实现跨域代理，原理大致与nginx相同，都是通过启一个代理服务器，实现数据的转发，也可以通过设置cookieDomainRewrite参数修改响应头中cookie中域名，实现当前域的cookie写入，方便接口登录认证。

1、nodejs服务器代理
使用node + express + http-proxy-middleware搭建一个proxy服务器。

```bash
npm i express htttp-proxy-middleware
```

```js
const express=require('express')
const app=express()
app.listen(5000)
const httpProxyMiddleware=require('http-proxy-middleware')
// 服务器代理  ---接口中间层 代理层
app.use('/api' ,httpProxyMiddleware.createProxyMiddleware({
    // 代理的地址
    target:'<http://localhost:8989>',
    // 默认false不修改。修改代理请求是他的主机名
    changeOrigin:true,
    // 修改响应头信息，实现跨域并允许带cookie
    onProxyRes: function(proxyRes, req, res) {
        res.header('Access-Control-Allow-Origin', '<http://localhost:5000>');
        res.header('Access-Control-Allow-Credentials', 'true');
    },

    // 匹配规则
    pathRewrite:{
        // 访问路径 映射到 目标服务器中的路径
        '^/v1/api':'/'
    }
}))
```

2、vue框架的跨域
vue中实现开发环境的时的反向代理进行跨域解决，在项目根目录下面创建一个vue.config.js文件，写下如下代码
vue.config.js部分配置：

```js
module.exports={
    // 指定服务器模块
    devServer:{
        // 代理
        proxy:{
            '/v1/api':{
                // 目标地址
                target:'<http://localhost:3000>',
                changeOrigin:true,
                pathRewrite:{
                    '/v1/api':'/api'
                }
            }
        }
    }
}
```

五、document.domain + iframe跨域
前提条件
这两个域名必须属于同一个一级域名!而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域。
Javascript出于对安全性的考虑，而禁止两个或者多个不同域的页面进行互相操作。
而相同域的页面在相互操作的时候不会有任何问题。

```js
alert(document.domain = "baidu.com");     //"baidu.com"
alert(document.domain = "www.baidu.com"); //"www.baidu.com"
```

举例
1）父窗口：`(<http://father.baidu.com/a.html>)`

```html
<iframe id="iframe" src="http://child.baidu.com/b.html"></iframe>
<script>
    document.domain = 'baidu.com';
    var user = 'admin';
</script>
```

预览
1）子窗口：`(http://child.baidu.com/b.html)`

```html
<script>
    document.domain = 'baidu.com';
    // 获取父窗口中变量
    console.log('get js data from parent ---> ' + window.parent.user);
</script>
```

六、location.hash + iframe跨域
hash 属性是一个可读可写的字符串，该字符串是 URL 的锚部分（从 # 号开始的部分）。

实现原理
a想要与b跨域相互通信，通过中间页c来实现。
三个页面，不同域之间利用iframe的location.hash传值，相同域之间直接js访问来通信。
利用location.hash传值,创建定时器，坚持hash的变化，执行相应的操作。
下面我们来完成一个案例：
具体实现
A域：a.html -> B域：b.html -> A域：c.html
a与b不同域只能通过hash值单向通信，b与c也不同域也只能单向通信，但c与a同域，所以c可通过parent.parent访问a页面所有对象。
1）a.html：`(<http://www.baidu1.com/a.html>)`

```html
<iframe id="iframe" src="http://www.baidu2.com/b.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');

    // 向b.html传hash值
    setTimeout(function() {
        iframe.src = iframe.src + '#user=admin';
    }, 1000);
    
    // 开放给同域c.html的回调方法
    function onCallback(res) {
        alert('data from c.html ---> ' + res);
    }
</script>
```

2）b.html：`(http://www.baidu2.com/b.html)`

```html
<iframe id="iframe" src="http://www.baidu1.com/c.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');

    // 监听a.html传来的hash值，再传给c.html
    window.onhashchange = function () {
        iframe.src = iframe.src + location.hash;
    };
</script>
3）c.html：(http://www.baidu1.com/c.html)

<script>
    // 监听b.html传来的hash值
    window.onhashchange = function () {
        // 再通过操作同域a.html的js回调，将结果传回
        window.parent.parent.onCallback('hello: ' + location.hash.replace('#user=', ''));
    };
</script>
```

优缺点
location.hash + iframe跨域的优点：

可以解决域名完全不同的跨域
可以实现双向通讯
location.hash + iframe跨域的缺点：

location.hash会直接暴露在URL里，并且在一些浏览器里会产生历史记录，数据安全性不高也影响用户体验
另外由于URL大小的限制，支持传递的数据量也不大。
七、window.name + iframe解决跨域
window.name属性的独特之处：只要在一个window下，无论url怎么变化，只要设置好了window.name，那么后续就一直都不会改变。同理，在iframe中，即使url在变化，iframe中的window.name也是一个固定的值，利用这个，我们就可以实现跨域了（2MB）。

举例
test1.html

```html
<body>
  <h2>test1页面</h2>
  <iframe src="http://192.168.0.1/php_demo/test2.html" frameborder="1"></iframe>
  <script>
    var ifr = document.querySelector('iframe')
    ifr.style.display = 'none'
    var flag = 0;
    ifr.onload = function () {
        console.log('跨域获取数据', ifr.contentWindow.name);
        ifr.contentWindow.close();
    }
  </script>
</body>
```

test2.html

```html
<body>
  <h2>test2页面</h2>
  <script>
    var person = {
      name: '大鹏_yp',
      age: 24,
      school: 'lngydx'
    }
    window.name = JSON.stringify(person)
  </script>
</body>
```

通过iframe的src属性由外域转向本地域，跨域数据即由iframe的window.name从外域传递到本地域。这个就巧妙地绕过了浏览器的跨域访问限制，但同时它又是安全操作。

八、postMessage通信跨域
PWA渐进式web应用

在HTML5中新增了postMessage方法，postMessage可以实现跨文档消息传输（Cross Document Messaging）
该方法可以通过绑定window的message事件来监听发送跨文档消息传输内容。
它可用于解决以下方面的问题：

a. 页面和其打开的新窗口的数据传递
b. 多窗口之间消息传递
c. 页面与嵌套的iframe消息传递
d. 上面三个场景的跨域数据传递

postMessage用法：

postMessage(data,origin)方法接受两个参数
参数说明：

data： html5规范支持任意基本类型或可复制的对象，但部分浏览器只支持字符串，所以传参时最好用JSON.stringify()序列化。
origin： 协议+主机+端口号，也可以设置为"*"，表示可以传递给任意窗口，如果要指定和当前窗口同源的话设置为"/"。
举例
postMessage：发送
onmessage：接收
1）a.html：`(<http://www.baidu1.com/a.html>)`

```html
<iframe id="iframe" src="http://www.baidu2.com/b.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');
    iframe.onload = function() {
        var data = {
            name: 'aym'
        };
        // 向domain2传送跨域数据
        iframe.contentWindow.postMessage(JSON.stringify(data), 'http://www.baidu2.com');
    };

    // 接受baidu2返回数据
    window.addEventListener('message', function(e) {
        alert('data from baidu2 ---> ' + e.data);
    }, false);
</script>
```

2）b.html：`(http://www.baidu2.com/b.html)`

```html
<script>
    // 接收baidu1的数据
    window.addEventListener('message', function(e) {
        alert('data from baidu1 ---> ' + e.data);

        var data = JSON.parse(e.data);
        if (data) {
            data.number = 16;

            // 处理后再发回baidu1
            window.parent.postMessage(JSON.stringify(data), 'http://www.baidu1.com');
        }
    }, false);
</script>
```

九、WebSocket协议跨域
WebSocket protocol是HTML5一种新的协议。它实现了浏览器与服务器全双工通信，同时允许跨域通讯，是server push技术的一种很好的实现。
原生WebSocket API使用起来不太方便，我们使用Socket.io，它很好地封装了webSocket接口，提供了更简单、灵活的接口，也对不支持webSocket的浏览器提供了向下兼容。
WebSocket 如何工作
Web浏览器和服务器都必须实现 WebSockets 协议来建立和维护连接。由于 WebSockets 连接长期存在，与典型的HTTP连接不同，对服务器有重要的影响。

基于多线程或多进程的服务器无法适用于 WebSockets，因为它旨在打开连接，尽可能快地处理请求，然后关闭连接。任何实际的 WebSockets 服务器端实现都需要一个异步服务器。

案例
1）前端代码：

```html
<div>user input：<input type="text"></div>
<script src="https://cdn.bootcss.com/socket.io/2.2.0/socket.io.js"></script>
<script>
var socket = io('http://www.baidu2.com:8080');

// 连接成功处理
socket.on('connect', function() {
    // 监听服务端消息
    socket.on('message', function(msg) {
        console.log('data from server: ---> ' + msg);
    });

    // 监听服务端关闭
    socket.on('disconnect', function() { 
        console.log('Server socket has closed.'); 
    });
});

document.getElementsByTagName['input'](0).onblur = function() {
    socket.send(this.value);
};
</script>
```

2）Nodejs socket后台：

```js
var http = require('http');
var socket = require('socket.io');

// 启http服务
var server = http.createServer(function(req, res) {
    res.writeHead(200, {
        'Content-type': 'text/html'
    });
    res.end();
});

server.listen('8080');
console.log('Server is running at port 8080...');

// 监听socket连接
socket.listen(server).on('connection', function(client) {
    // 接收信息
    client.on('message', function(msg) {
        client.send('hello：' + msg);
        console.log('data from client: ---> ' + msg);
    });

    // 断开处理
    client.on('disconnect', function() {
        console.log('Client socket has closed.'); 
    });
});
```
