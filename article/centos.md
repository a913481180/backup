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
源码的安装一般由3个步骤组成：

配置(configure)
编译(make)
安装(make install)。
configure文件是一个可执行的脚本文件，它有很多选项，在待安装的源码目录下使用命令./configure –help可以输出详细的选项列表。

其中--prefix选项是配置安装目录，如果不配置该选项，安装后可执行文件默认放在/usr/local/bin，库文件默认放在/usr/local/lib，

配置文件默认放在/usr/local/etc，其它的资源文件放在/usr/local/share，比较凌乱。

如果配置了--prefix，如：

./configure --prefix=/usr/local/test
安装后的所有资源文件都会被放在/usr/local/test目录中，不会分散到其他目录。

使用--prefix选项的另一个好处是方便卸载软件或移植软件

当某个安装的软件不再需要时，只须简单的删除该安装目录，就可以把软件卸载干净；
移植软件只需拷贝整个目录到另外一个机器即可（相同的操作系统下）
当然要卸载程序，也可以在原来的make目录下用一次make uninstall，但前提是Makefile文件有uninstall命令。

二、关于卸载
如果没有配置--prefix选项，源码包也没有提供make uninstall，则可以通过以下方式可以完整卸载：

一个临时目录重新安装一遍，如：

  ./configure --prefix=/tmp/to_remove && make install
然后遍历/tmp/to_remove的文件，如vim的/bin/vimdiff =>find /usr/ -name vimdiff

删除对应安装位置的文件即可（因为/tmp/to_remove里的目录结构就是没有配置--prefix选项时的目录结构）。