---
title: nginx
date: 2022-08-11 20:22:22
categories:
  - linux
---

# Nginx

> 负载均衡、反向代理、动静结合

## 配置文件匹配顺序

`/etc/nginx/conf.d/`多个配置文件时，会优先匹配配置了`server_name`且值不是`localhost`，接着是`localhost`，再接着是没有配置的。若端口号和`server_name`都一样则优先选择该目录下的第一个配置文件。

## 常用命令

```bash
nginx -v
nginx -V
nginx -t //查看配置是否有问题
nginx -s reload //重新加载配置
nginx -s stop //强行停止
nginx -s quit //安全退出
ps -ef|grep nginx //查看进程
systemctl enable nginx //开机自动启动
//防火墙
systemctl status firewalld.service
systemctl stop firewalld.service
systemctl disable firewalld.service
systemctl status firewalld.service
```

## 开启 GZIP 压缩

```bash
server{
    gzip on;
    gzip_types application/javascript;#压缩类型，文本文件压缩效果最好,图片视频资源压缩作用不大，大文件资源会消耗大量cpu资源不推荐
    gzip_min_length 1k;#最小压缩单位，小于1k压缩意义不大
    gzip_comp_level 5;#压缩级别，1-9，数字越大压缩效果越好，但会加大cpu压力，高并发场景不建议调太高,建议5左右
    gzip_vary on;#在响应头添加very:accept-encoding,让代理服务器根据请求头识别是否启用了gzip压缩
    gzip_http_version 1.1;#启用gzip压缩的最低http版本，默认1.1
    gzip_buffers 4 4k;#设置压缩所需的缓冲区大小和申请内存的倍率，如4 4k代表以4k为单位，按照原始数据大小以4k为单位的4倍申请内存。默认是申请跟原始数据相同大小的内存空间。
    gzip_static on;#静态压缩，若提前已准备好了压缩文件.gz的压缩包或者提供静态文件服务，可以启用。它可避免动态压缩，提高性能
    gzip_proxied off;#nginx作为反向代理时，开启或关闭后端服务器返回的结果（前提是后端必须返回包含Via的请求头）。off-关闭所有代理结果数据的压缩。expired-若请求头包含‘Expires’信息，则启用。no-cache-若请求头包含‘Cache-Control:no-cache’信息，则启用。no-store-若请求头包含‘Cache-Control:no-store’信息，则启用。private->若请求头包含‘Cache-Control:private’信息，则启用。no_last_modified->若请求头不包含‘Last-Modified’信息，则启用。no_etag->若请求头不包含‘Etag’信息，则启用。auth->若请求头包含“Authorization"信息，则启用。any->无条件启用压缩
    gzip_disable MSIE[1-6]\.;#设置禁用浏览器进行Gzip压缩，ie6对gzip压缩支持不好，会造成假死。
    listen 8080;
    server_name localhost;
    location / {
        root /opt/test/;
    }
}
```

## 开启 Brotli

> 比 gzip 性能高，ie 浏览器不支持，仅支持 https，无法使用时，会降级为 gzip

1. 下载源码
2. 下载算法
3. 重新编译，`./configure --prefix=安装目录 --with-http_ssl_module --add-module=brotli的安装目录`
4. 安装好后，将 objs 文件夹中 nginx 执行文件替换掉 sbin 中的执行文件
5. 编辑文件

```bash
server{
    ...
brotli on；
brotli_type application/javascript;
}
```

## 请求限流

```bash
语法：limit_req zone=name [burst=number][nodelay|delay=number];
上下文：http,server,location
```

示例：

```bash
http:{
limit_req_zone $binary_remote_addr zone=ip_limit:10m rate=1r/s;
...
}
```

`$binary_remote_addr`->通过 remote_addr 这个标识来做限制，限制的是同一客户端的 ip 地址，在这里，客户端 ip 地址作为关键。注意，不是`$ remote_addr`,`$ remote_addr`变量的大小可以从 7 到 15 个字节不等，存储状态在 32 位平台上占用 32 或 64 字节，在 64 位平台上总是 64 字节。对于 ipv4 地址，`$binary_remote_addr`变量始终是 4 个字节，对于 ipv6 地址则为 16 个字节，
`zone=ip_limit:10m`->生成一个 10MB 大小，名字为 ip_limitd 的内存区域，主要用来存储访问频次信息。
`rate=1r/s`->标识允许相同标识客户端的一个访问频次，1r 即 1 次

```bash
server{
    ...
location / {
limit_req zone=ip_limit burst=5 nodelay;
...
}}
```

`zone`->表示使用哪个区域来做限制
`burst`->设置了一个大小为 5 的缓存区，当有大量请求即 burst 时，超过访问频次限制的请求可以先放到此缓存区。
`nodelay`->超过访问频次且缓存区也满了的时候，直接返回 503，若无设置，则请求会等待排队。

## 连接限流

```bash
语法：limit_conn zone number;
上下文：http,server,location
```

示例：

```bash
http:{
limit_conn_zone $binary_remote_addr zone=addr:10m;
...
}
```

`$binary_remote_addr`->通过 remote_addr 这个标识来做限制，限制的是同一客户端的 ip 地址.
`zone=addr:10m`->生成一个 10MB 大小，名字为 addr 的内存区域，主要用来存储访问频次信息。

```bash
server{
...
location / {
limit_conn addr 1;
...
}
}

```

`zone`->表示使用哪个区域来做限制
`1`->允许相同标识客户端的一个访问频次，1 代表只允许每个 ip 地址 1 个连接。

## 防盗链

```bash

server{
listen 8080;
server_name localhost;
location / {
root /opt/test/;
}
location ~_ ._\.(gif|jpg|png)${
        root /opt/test/img/;
        valid_referers none blocked xxx.com;
        if($invalid_referer){
#return 403;返回错误页面
rewrite ^/ http://xxxx/error;
break;
}
}

}

```

破解方法

```html
<header>
  <meta name="referrer" content="no-referrer" />
</header>
```

## 如何利用 Nginx 代理获取真实 IP

```bash

server {
listen 80;
server_name xxx.xxxx.com;

location / { #保留代理之前的 host 包含客户端真实的域名和端口号
proxy_set_header Host $host; #保留代理之前的真实客户端 ip
proxy_set_header X-Real-IP $remote_addr;
 #这个 Header 和 X-Real-IP 类似，但它在多级代理时会包含真实客户端及中间每个代理服务器的 IP
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; #表示客户端真实的协议（http 还是 https）
proxy_set_header X-Forwarded-Proto $scheme; #指定修改被代理服务器返回的响应头中的 location 头域跟 refresh 头域数值 #如果使用"default"参数，将根据 location 和 proxy_pass 参数的设置来决定。
#proxy_redirect [ default|off|redirect replacement ];
proxy_redirect off;
proxy_pass http://IP:PORT;
}
}

```

经过反向代理后，由于在客户端和 web 服务器之间增加了中间层，因此 web 服务器无法直接拿到客户端的 ip，通过`$remote_addr`变量拿到的将是反向代理服务器的 ip 地址。

这句话的意思是说，当你使用了 nginx 反向服务器后，在 web 端使用 `request.getRemoteAddr()`（本质上就是获取`$remote_addr`），取得的是 nginx 的地址，即`$remote_addr`变量中封装的是 nginx 的地址，当然是没法获得用户的真实 ip 的。但是，nginx 是可以获得用户的真实 ip 的，也就是说 nginx 使用`$remote_addr`变量时获得的是用户的真实 ip，如果我们想要在 web 端获得用户的真实 ip，就必须在 nginx 里作一个赋值操作，即我在上面的配置：

`proxy_set_header X-Real-IP $remote_addr;`。

`$remote_addr` 只能获取到与服务器本身直连的上层请求 ip，所以设置`$remote_addr` 一般都是设置第一个代理上面;

但是问题是，有时候是通过 cdn 访问过来的，那么后面 web 服务器获取到的，永远都是 cdn 的 ip 而非真是用户 ip,那么这个时候就要用到`X-Forwarded-For`了，这个变量的意思，其实就像是链路反追踪，从客户的真实 ip 为起点，穿过多层级的 proxy ，最终到达 web 服务器，都会记录下来，所以在获取用户真实 ip 的时候，一般就可以设置成下面的配置：

`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`。

`X-Forwarded-For`变量，这是一个 squid 开发的，用于识别通过 HTTP 代理或负载平衡器原始 IP 一个连接到 Web 服务器的客户机地址的非 rfc 标准，如果有做`X-Forwarded-For`设置的话,每次经过 proxy 转发都会有记录,格式就是`client1,proxy1,proxy2`以逗号隔开各个地址，由于它是非 rfc 标准，所以默认是没有的，需要强制添加。在默认情况下经过 proxy 转发的请求，在后端看来远程地址都是 proxy 端的 ip 。也就是说在默认情况下我们使用`request.getAttribute(“X-Forwarded-For”)`获取不到用户的 ip，如果我们想要通过这个变量获得用户的 ip，我们需要自己在 nginx 添加配置：

`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`

它意思是增加一个`$proxy_add_x_forwarded_for`到`X-Forwarded-For`里去，注意是增加，而不是覆盖，当然由于默认的`X-Forwarded-For`值是空的，所以我们总感觉`X-Forwarded-For`的值就等于`$proxy_add_x_forwarded_for`的值，实际上当你搭建两台 nginx 在不同的 ip 上，并且都使用了这段配置，那你会发现在 web 服务器端通过 `request.getAttribute(“X-Forwarded-For”)`获得的将会是客户端 ip 和第一台 nginx 的 ip。

那么`$proxy_add_x_forwarded_for`又是什么？

`$proxy_add_x_forwarded_for`变量包含客户端请求头中的`X-Forwarded-For`与`$remote_addr`两部分，他们之间用逗号分开。

举个例子，有一个 web 应用，在它之前通过了两个 nginx 转发，www.linuxidc.com即用户访问该web通过两台nginx。

在第一台 nginx 中,使用：

`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`

现在的`$proxy_add_x_forwarded_for`变量的`X-Forwarded-For`部分是空的，所以只有`$remote_addr`，而`$remote_addr`的值是用户的 ip，于是赋值以后，`X-Forwarded-For`变量的值就是用户的真实的 ip 地址了。

到了第二台 nginx，使用：

`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`

现在的`$proxy_add_x_forwarded_for`变量，`X-Forwarded-For`部分包含的是用户的真实 ip，`$remote_addr`部分的值是上一台 nginx 的 ip 地址，于是通过这个赋值以后现在的`X-Forwarded-For`的值就变成了“用户的真实 ip，第一台 nginx 的 ip”，这样就清楚了吧。

## 部署前端项目

```conf
server{
  listen 8080;#监听的端口
  location /{
    try_files $uri $uri/ /index.html;
    root  /home/build;##/home/build为项目打包文件所在的目录
  }

  location /apis{
    rewrite ^.+apis/?(.*)$ $1 break;##除去/apis
    proxy_pass http://192.168.2.16:8080;##接口代理到服务器
    ## 真实ip
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    ## 504 Gateway Time-out
    proxy_connect_timeout 300s;
    proxy_send_timeout 300s;
    proxy_read_timeout 300s;
    ## 日志
    #access_log /var/log/nginx/access.foo.log main;
    #error_log /var/log/nginx/error.foo.log;
  }
  client_max_body_size 10m;#Nginx默认的request body为1M
  gzip on;#开启gzip压缩
  gzip_min_length 1k;#大于1k的文件才压缩
  gzip_types text/plain text/css text/javascript application/xml application/x-javascript application/javascript;#压缩类型，文本文件压缩效果最好,图片视频资源压缩作用不大，大文件资源会消耗大量cpu资源不推荐
  gzip_disable MSIE[1-6]\.;#设置禁用浏览器进行Gzip压缩，ie6对gzip压缩支持不好，会造成假死。
  gzip_vary on;#在响应头添加very:accept-encoding,让代理服务器根据请求头识别是否启用了gzip压缩
}
```

## 常见错误

### 403 Forbidden

- 资源路径是否填写正确
- nginx 的启动用户要和当前用户一致
编辑`/etc/nginx/nginx.conf`

```diff
...
- user nginx;
+ user root;
...

```

### 反向代理 504 Gateway Time-out

```
  proxy_connect_timeout 300s;
  proxy_send_timeout 300s;
  proxy_read_timeout 300s;
```
