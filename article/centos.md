---
title: Centos7安装 
date: 2022-01-11 20:22:22
categories:
- linux
---


### could not retrieve mirrorlist解决

在执行yum指令的时候出现这个问题,先尝试一下ping指令:

```
ping 127.0.0.1
ping www.baidu.com
```

如果第一个正常,第二个错误,那就可能是你没有IP或者你没有启用联网功能;
没有IP就查查怎么配静态或者动态IP;
`sudo vi /etc/sysconfig/network-scripts/ifcfg-ens33`
修改配置`ONBOOT=no`----->`ONBOOT=yes`
wq保存退出
`service network restart`重启网络服务

如果你能ping通百度,还是报这个问题,这个就可能是你的DNS解析不了你的请求,修改DNS啊
```
vi /etc/resolv.conf
nameserver 改成 8.8.8.8
```
### 启用ifconfig 命令

依赖于 net-tools 软件

` yum install -y net-tools`

CentOS7最小化安装后没有wget软件，但是以后我们会经常用到这个组件，所以我们安装一下

` yum install -y wget `

### CentOS自带的国外源有时候会很慢，替换成国内的阿里源

先进入源的目录 
`cd /etc/yum.repos.d `
备份一下官方源 
`mv CentOS-Base.repo CentOS-Base.repo.bak `
将阿里源文件下载下来 
`wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo `
重建源数据缓存 
`yum clean all`     # 清除系统所有的yum缓存
`yum makecache `# 生成yum缓存
ok,换源完成

### 在某些特定的情况下，执行./configure的时候会报错：

error:newly created file is older than distributed files!
这是由于configure文件时间大于当前系统时间。

解决方法：
```
方法一：
cp configure configure_bak
rm -f configure
cp configure_bak configure

方法二：修改当前系统时间
hwclock --set --date="月/日/年 小时：分钟：秒钟"
hwclock --hctosys
```


### 一、正常的编译安装/卸载
源码的安装一般由3个步骤组成：配置(configure)、编译(make)、安装(make install)。
configure文件是一个可执行的脚本文件，它有很多选项，在待安装的源码目录下使用命令`./configure –help`可以输出详细的选项列表。

其中`--prefix`选项是配置安装目录，如果不配置该选项，安装后可执行文件默认放在`/usr/local/bin`，库文件默认放在`/usr/local/lib`，

配置文件默认放在`/usr/local/etc`，其它的资源文件放在`/usr/local/share`，比较凌乱。

如果配置了`--prefix`，如：

`./configure --prefix=/usr/local/test`
安装后的所有资源文件都会被放在`/usr/local/test`目录中，不会分散到其他目录。

使用`--prefix`选项的另一个好处是方便卸载软件或移植软件

当某个安装的软件不再需要时，只须简单的删除该安装目录，就可以把软件卸载干净；
移植软件只需拷贝整个目录到另外一个机器即可（相同的操作系统下）
当然要卸载程序，也可以在原来的`make`目录下用一次`make uninstall`，但前提是`Makefile`文件有`uninstall`命令。

### 二、关于卸载
如果没有配置`--prefix`选项，源码包也没有提供`make uninstall`，则可以通过以下方式可以完整卸载：

一个临时目录重新安装一遍，如：

  `./configure --prefix=/tmp/to_remove && make install`
然后遍历`/tmp/to_remove`的文件，如vim的`/bin/vimdiff =>find /usr/ -name vimdiff`

删除对应安装位置的文件即可（因为`/tmp/to_remove`里的目录结构就是没有配置`--prefix`选项时的目录结构）。


## CentOS离线状态下安装Python3.7.0
- python安装包：Python-3.7.0
- 依赖安装包
zlib-devel　　　　　krb5-devel     　　　　ibselinux-devel
bzip2-devel　　　　openssl-devel　　　　ncurses-devel 　　　
sqlite-devel   　　　 readline-devel　　　  tk-devel      
gdbm-devel     　　  db4-devel　　　　　   libpcap-devel 　　　　xz-devel     
- 解压python安装包: tar -xvJf  Python-3.7.0.tar.xz

- 编译安装
```
mkdir /usr/local/python3 # 创建编译安装目录
cd Python-3.7.0　　　　　　# 进入python的解压目录
./configure --prefix=/usr/local/python3
make && make install　　　# 编译$安装
```

- 创建软连接

```
ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3
```

- 验证是否安装成功
python3 -V
pip3 -V

## CentOS7离线安装Nginx

- 准备准备离线安装包

openssl

zlib

pcre

nginx
 
- rpm -ivh *.rpm --nodeps --force

- 安装nginx

```
下载解压

tar -xzvf  nginx-1.8.1.tar.gz

cd  nginx-1.8.1

./configure --prefix=/home/work/nginx

编译并安装

make && make install

运行nginx
cd //home/work/nginx/sbin
./nginx
```

- 防火墙
```
[root@rhel7 ~]# systemctl status firewalld.service
[root@rhel7 ~]# systemctl stop firewalld.service
[root@rhel7 ~]# systemctl disable firewalld.service
[root@rhel7 ~]# systemctl status firewalld.service
```
- Nginx相关命令

4.1 版本查看
```
[root@nginx ~]# nginx  -v
nginx version: nginx/1.12.2
```
4.2 查看加载的模块
```
[root@nginx ~]# nginx -V
nginx version: nginx/1.12.2
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-28) (GCC) 
configure arguments: --add-module=/root/nginx-sticky-1.2.5/
```.
4.3 启停命令

4.3.1 启动
```
[root@nginx nginx-1.12.2]# nginx
```
4.3.2 停止
```
[root@nginx nginx-1.12.2]# nginx -s stop
[root@nginx nginx-1.12.2]# nginx -s quit
```

4.3.3 动态加载
```
[root@nginx nginx-1.12.2]# ngins -s reload
```
4.3.4 测试配置文件nginx.conf正确性
```
[root@nginx ~]# nginx  -t
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
```
`nginx -s stop`:此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程。

`nginx -s reload`:动态加载，当配置文件nginx.conf有变化时执行该命令动态加载。

4.4 开机自启动

编辑/etc/rc.d/rc.local文件，新增一行/usr/local/nginx/sbin/nginx
```
[root@nginx rc.d]# cd /etc/rc.d
[root@nginx rc.d]# sed -i '13a /usr/local/nginx/sbin/nginx' /etc/rc.d/rc.local 
[root@nginx rc.d]# chmod u+x rc.local
```
