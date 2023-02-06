---
title: nginx
date: 2022-08-11 20:22:22
categories:
- linux
---

- 常用命令
```
nginx -v
nginx -V
nginx -t //查看配置是否有问题
nginx -s reload //重新加载配置
nginx -s stop //强行停止
nginx -s quit //安全退出
ps -ef|grep nginx //查看进程
systemctl enable nginx //开机自动启动
//防火墙
[root@rhel7 ~]# systemctl status firewalld.service
[root@rhel7 ~]# systemctl stop firewalld.service
[root@rhel7 ~]# systemctl disable firewalld.service
[root@rhel7 ~]# systemctl status firewalld.service
```

- 开启GZIP压缩
```
server{
    gzip on;
    gzip_type application/javascript;#压缩类型，文本文件压缩效果最好,图片视频资源压缩作用不大，大文件资源会消耗大量cpu资源不推荐
    gzip_min_length 1k;#最小压缩单位，小于1k压缩意义不大
    gzip_comp_level 5;#压缩级别，1-9，数字越大压缩效果越好，但会加大cpu压力，高并发场景不建议调太高,建议5左右
    gzip_very on;#在响应头添加very:accept-encoding,让代理服务器根据请求头识别是否启用了gzip压缩
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

- 开启Brotli
>比gzip性能高，ie浏览器不支持，仅支持https，无法使用时，会降级为gzip
- 下载源码
- 下载算法
- 重新编译，`./configure --prefix=安装目录 --with-http_ssl_module --add-module=brotli的安装目录`
- 安装好后，将 objs文件夹中nginx执行文件替换掉sbin中的执行文件
- 编辑文件
```
server{
    ...
brotli on；
brotli_type application/javascript;
}
```

 

- 防盗链
```
server{
    listen 8080;
    server_name localhost;
    location / {
        root /opt/test/;
    }
    location ~* .*\.(gif|jpg|png)${
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
```
<header>
<meta name="referrer" content='no-referrer' />
</header>

```

- 如何利用Nginx代理获取真实IP
```
server {
   listen 80;
   server_name xxx.xxxx.com;
  
   location / {
       #保留代理之前的host 包含客户端真实的域名和端口号
       proxy_set_header    Host  $host; 
       #保留代理之前的真实客户端ip
       proxy_set_header    X-Real-IP  $remote_addr;  
       #这个Header和X-Real-IP类似，但它在多级代理时会包含真实客户端及中间每个代理服务器的IP
       proxy_set_header    X-Forwarded-For  $proxy_add_x_forwarded_for;
       #表示客户端真实的协议（http还是https）
       proxy_set_header X-Forwarded-Proto $scheme;
       #指定修改被代理服务器返回的响应头中的location头域跟refresh头域数值
       #如果使用"default"参数，将根据location和proxy_pass参数的设置来决定。
       #proxy_redirect [ default|off|redirect replacement ];
       proxy_redirect off;
       proxy_pass http://IP:PORT;
   }
}
```
经过反向代理后，由于在客户端和web服务器之间增加了中间层，因此web服务器无法直接拿到客户端的ip，通过`$remote_addr`变量拿到的将是反向代理服务器的ip地址。
这句话的意思是说，当你使用了nginx反向服务器后，在web端使用request.getRemoteAddr()（本质上就是获取`$remote_addr`），取得的是nginx的地址，即`$remote_addr`变量中封装的是nginx的地址，当然是没法获得用户的真实ip的。但是，nginx是可以获得用户的真实ip的，也就是说nginx使用`$remote_addr`变量时获得的是用户的真实ip，如果我们想要在web端获得用户的真实ip，就必须在nginx里作一个赋值操作，即我在上面的配置：
`proxy_set_header X-Real-IP $remote_addr;`
$remote_addr 只能获取到与服务器本身直连的上层请求ip，所以设置`$remote_addr` 一般都是设置第一个代理上面;但是问题是，有时候是通过cdn访问过来的，那么后面web服务器获取到的，永远都是cdn 的ip 而非真是用户ip,那么这个时候就要用到`X-Forwarded-For` 了，这个变量的意思，其实就像是链路反追踪，从客户的真实ip为起点，穿过多层级的proxy ，最终到达web 服务器，都会记录下来，所以在获取用户真实ip的时候，一般就可以设置成 下面的配置
`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; `
`X-Forwarded-For`变量，这是一个squid开发的，用于识别通过HTTP代理或负载平衡器原始IP一个连接到Web服务器的客户机地址的非rfc标准，如果有做`X-Forwarded-For`设置的话,每次经过proxy转发都会有记录,格式就是client1,proxy1,proxy2以逗号隔开各个地址，由于它是非rfc标准，所以默认是没有的，需要强制添加。在默认情况下经过proxy转发的请求，在后端看来远程地址都是proxy端的ip 。也就是说在默认情况下我们使用request.getAttribute(“X-Forwarded-For”)获取不到用户的ip，如果我们想要通过这个变量获得用户的ip，我们需要自己在nginx添加配置：`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`
意思是增加一个`$proxy_add_x_forwarded_for`到`X-Forwarded-For`里去，注意是增加，而不是覆盖，当然由于默认的`X-Forwarded-For`值是空的，所以我们总感觉`X-Forwarded-For`的值就等于`$proxy_add_x_forwarded_for`的值，实际上当你搭建两台nginx在不同的ip上，并且都使用了这段配置，那你会发现在web服务器端通过request.getAttribute(“X-Forwarded-For”)获得的将会是客户端ip和第一台nginx的ip。

那么`$proxy_add_x_forwarded_for`又是什么？
`$proxy_add_x_forwarded_for`变量包含客户端请求头中的`X-Forwarded-For`与`$remote_addr`两部分，他们之间用逗号分开。

举个例子，有一个web应用，在它之前通过了两个nginx转发，www.linuxidc.com即用户访问该web通过两台nginx。

在第一台nginx中,使用：
`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`
现在的`$proxy_add_x_forwarded_for`变量的`X-Forwarded-For`部分是空的，所以只有`$remote_addr`，而`$remote_addr`的值是用户的ip，于是赋值以后，`X-Forwarded-For`变量的值就是用户的真实的ip地址了。

到了第二台nginx，使用：

`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`

现在的`$proxy_add_x_forwarded_for`变量，`X-Forwarded-For`部分包含的是用户的真实ip，`$remote_addr`部分的值是上一台nginx的ip地址，于是通过这个赋值以后现在的`X-Forwarded-For`的值就变成了“用户的真实ip，第一台nginx的ip”，这样就清楚了吧。