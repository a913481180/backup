---
title: AJax
date: 2020-11-12 20:11:33
catergories:
- study
---

# Ajax

`XMLHttpRequest`IE8以下不兼容；IE8以下声明ajax方法为`ActiveXObject("Microsoft.XMLHTTP");`

1. 创建ajax对象
```
var xhr=null
if(window.XMLHttpRequest){
 xhr=new XMLHttpRequest();
}else{
xhr=new ActiveXObject("Microsoft.XMLHTTP");
}
```

2. 调用open

```
/*
第一个参数： 请求方式 
第二个：url
第三个：是否异步，true异步，false同步
*/
xhr.open("get","./1.txt",true);
//或
xhr.open("get","./get.php?name=yyyy&age=22&password=342",true);
```

3. 调用send

```
xhr.send();
//post方法，将数据放在send()里提交
xhr.setRequestHeader('content-type','application/x-www-form-urlencoded');	//声明发送的数据类型
xhr.sen("name=yyy&age=33&password=124564");	//无需编码
//post没有缓存问题
```

4. 等待数据响应

onreadystatechange事件

| readyState属性|请求状态|
|-|-|
|0|（初始化）还没调用open（）方法|
|1|（载入）已调用send()方法。正在发送请求|
|2|(载入完成）send()方法完成，已收到全部响应内容|
|3|(解析）正在解析响应内容|
|4|(完成）响应内容解析完成，可在客户端调用|

|status属性|服务器（请求资源）的状态|
|-|-|
|200||
|400||


`responseText`:返回以文本形式存放的内容;
`responseXML`:返回XML形式的内容

```
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

>ajax 只能下载同源的数据，跨源的数据禁止下载

- 同源策略

   1. 同协议
   2. 同域名/IP
   3. 同端口号

同源策略禁止跨源请求


- 跨源方法：
   1. 修改ajax同源协议（不建议）
   2. 委托php文件进行跨源
   3. JSONP

- JSONP跨域的使用流程
   1. 先去声明一个函数，这个函数有一个形参，这个形参会拿到我们想要下载的数据，
   2. 在需要下载数据的时候，动态创建script标签，将标签src属性设置成下载数据的链接
   3. 当script插入到页面的时候，就会调用号已经封装好的函数，将数据传过来

```
<script>
function download(data){
console.log(data);
}
</script>
<script src="test.js"></script>
```

- test.js

```
download("hello!!!!!!!!!");
```

### 表单提交

form表单点击提交数据后需跳转页面，ajax为异步进行的数据传输

- action:点击submit后跳转的url
- method:表单提交方式get/post
   - get(默认)
直接将数据拼接在url后面进行提交;用`？`进行拼接,多个数据之间用`&`进行连接
      - 优点：简单
      - 缺点：不安全，最大2kb，无法实现上传大文件


```
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
```
<form action="post.php" method="post"  enctype="application/x-www-from-urlencoded">
<input type="text" name="username" placeholder="name" />
<input type="text" name="age" placeholder="age" />
<input type="text" name="password" placeholder="" />
<input type"submit" value="post按钮"/>
</form>
```
php

```
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

```
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
