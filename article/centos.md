---
title: Centos7安装
date: 2022-01-10 20:22:22
categories:
  - linux
tags:
  - system
  - linux
---

### could not retrieve mirrorlist 解决

在执行 yum 指令的时候出现这个问题,先尝试一下 ping 指令:

```bash
ping 127.0.0.1
ping www.baidu.com
```

如果第一个正常,第二个错误,那就可能是你没有 IP 或者你没有启用联网功能;
没有 IP 就查查怎么配静态或者动态 IP;
`sudo vi /etc/sysconfig/network-scripts/ifcfg-ens33`
修改配置`ONBOOT=no`----->`ONBOOT=yes`
wq 保存退出
`service network restart`重启网络服务

如果你能 ping 通百度,还是报这个问题,这个就可能是你的 DNS 解析不了你的请求,修改 DNS 啊

```bash
vi /etc/resolv.conf
nameserver 改成 8.8.8.8
```

### 启用 ifconfig 命令

依赖于 net-tools 软件

` yum install -y net-tools`

CentOS7 最小化安装后没有 wget 软件，但是以后我们会经常用到这个组件，所以我们安装一下

`yum install -y wget`

### CentOS 自带的国外源有时候会很慢，替换成国内的阿里源

先进入源的目录
`cd /etc/yum.repos.d `
备份一下官方源
`mv CentOS-Base.repo CentOS-Base.repo.bak `
将阿里源文件下载下来
`wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo `
重建源数据缓存
`yum clean all` # 清除系统所有的 yum 缓存
`yum makecache `# 生成 yum 缓存
ok,换源完成

### 在某些特定的情况下，执行./configure 的时候会报错：

error:newly created file is older than distributed files!
这是由于 configure 文件时间大于当前系统时间。

解决方法：

```bash
#方法一：
cp configure configure_bak
rm -f configure
cp configure_bak configure

#方法二：修改当前系统时间
hwclock --set --date="月/日/年 小时：分钟：秒钟"
hwclock --hctosys
```

### 一、正常的编译安装/卸载

源码的安装一般由 3 个步骤组成：配置(configure)、编译(make)、安装(make install)。
configure 文件是一个可执行的脚本文件，它有很多选项，在待安装的源码目录下使用命令`./configure –help`可以输出详细的选项列表。

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
然后遍历`/tmp/to_remove`的文件，如 vim 的`/bin/vimdiff =>find /usr/ -name vimdiff`

删除对应安装位置的文件即可（因为`/tmp/to_remove`里的目录结构就是没有配置`--prefix`选项时的目录结构）。

### 安装桌面

1. yum 的 group 指令

yum 可以以程序组的模式来安装成套的软件包。支持的软件包可以通过，对于 CentOS 6，Desktop、Desktop Platform、KDE Desktop、X Window System 是主要的桌面环境。对于 CentOS 7，有 KDE Plasma Workspaces 和 Gnome Desktop 两大桌面环境。

2. 安装图形桌面环境

CENTOS 7
CentOS 7 上的桌面环境安装包组合进行了调整，比以前要简单了。主要有两大阵营，KDE 和 GNOME。

因为没时间测试，只是预估着下面的指令应该能完成任务，请注意！

要安装 KDE 桌面环境（使用的是 Plasma 作为默认的桌面管理器了，很漂亮，看这里），

复制代码代码如下:
`yum groupinstall "KDE Plasma Workspaces"`

要安装 GNOME 环境，

复制代码代码如下:
`yum groupinstall "GNOME Desktop"`

安装程序会自动解决安装包和组件的依赖关系。

CENTOS 6
要安装 KDE 桌面环境，执行指令，

复制代码代码如下:
`yum groupinstall "X Window System" "KDE Desktop" Desktop`

即可，同时安装了 3 个软件包。注意，因为 KDE Desktop 和 X Window System 两个软件包名称中间都包含空格，需要用引号引起来才行。

要安装 Gnome 桌面环境，执行指令，

复制代码代码如下:
` yum groupinstall "X Window System" "Desktop Platform" Desktop`

即可，也是同时安装了 3 个软件包，其中 X Window System 是必须的，不管是 Gnome 还是 KDE。

既然是桌面环境，可能还需要诸如字体、管理工具之类的，如，

复制代码代码如下:

```bsh
yum -y groupinstall "Graphical Administration Tools"
yum -y groupinstall "Internet Browser"
yum -y groupinstall "General Purpose Desktop"
yum -y groupinstall "Office Suite and Productivity"
yum -y groupinstall "Graphics Creation Tools"
```

3. 启用

从命令行直接启动图形桌面环境，

`startx`
这样就会启动默认的 Gnome 或者 KDE 桌面环境。如果有人喜欢同时安装 GNOME 和 KDE，切换方法可以参考 CentOS 文档。

如果希望启动时自动启动到图形桌面，需要修改启动配置。因为 CentOS 7 开始使用 systemd 管理器，其操作方式与之前版本有所不同。

CENTOS 7：
CentOS 7 中直接使用 systemd 指令修改启动目的状态即可。

使用，

复制代码代码如下:
`systemctl get-default`

可以查询到当前所设定的状态。multi-user.target 相当于以前的 level 3，也就是命令行终端；而 graphical.target 相当于以前的 level 5，也就是图形界面。

所以如果要设置默认启动到图形界面，则执行，

复制代码代码如下:
`systemctl set-default graphical.target `

CENTOS 6 等：
编辑 /etc/inittab，将 id:3:initdefault: 改为 id:5:initdefault:。（请注意这里的英文半角冒号。）参考这里。

直接用 sed 会很方便，

复制代码代码如下:
`sed -i 's/id:3:initdefault:/id:5:initdefault:/' /etc/inittab`

启动图形界面后，如果希望从图形界面切换到命令行界面，可以用 Ctrl + Alt + F6（实际上 F1 到 F6 都行，不过它们代表 Linux 中不同的控制台），或者反过来 Ctrl + Alt + F7 回到刚才的图形界面。

4.开机跳过图形化界面

复制代码代码如下:
`vi /etc/inittab--编辑/etc/inittab文件`

找到下面语句：

复制代码代码如下:

```
# Default runlevel. The runlevels used by RHS are:
# 0 - halt (Do NOT set initdefault to this)--停机
# 1 - Single user mode --单用户模式
# 2 - Multiuser, without NFS (The same as 3, if you do not havenetworking) --多用户模式，不支持NFS
# 3 - Full multiuser mode--多用户模式
# 4 - unused--没有使用
# 5 - X11--图形界面方式
# 6 - reboot (Do NOT set initdefault to this)--重新启动
#
id:5:initdefault:--默认运行等级是5，只要将此处改成 id:3:initdefault:即可
```

在文本模式想启动图形界面，可以打如下命令：

复制代码代码如下:
`#startx`

## CentOS 离线状态下安装 Python3.7.0

- python 安装包：Python-3.7.0
- 依赖安装包
  zlib-devel 　　　　　 krb5-devel 　　　　 ibselinux-devel
  bzip2-devel 　　　　 openssl-devel 　　　　 ncurses-devel 　　　
  sqlite-devel 　　　 readline-devel 　　　 tk-devel  
  gdbm-devel 　　 db4-devel 　　　　　 libpcap-devel 　　　　 xz-devel
- 解压 python 安装包: tar -xvJf Python-3.7.0.tar.xz

- 编译安装

```bash
mkdir /usr/local/python3 # 创建编译安装目录
cd Python-3.7.0　　　　　　# 进入python的解压目录
./configure --prefix=/usr/local/python3
make && make install　　　# 编译$安装
```

- 创建软连接

```bash
ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3
```

- 验证是否安装成功
  python3 -V
  pip3 -V

## CentOS7 离线安装 Nginx

- 准备准备离线安装包

`openssl`

`zlib`

`pcre`

`nginx`

- rpm -ivh \*.rpm --nodeps --force

- 安装 nginx

```bash
#下载解压

tar -xzvf  nginx-1.8.1.tar.gz

cd  nginx-1.8.1

./configure --prefix=/home/work/nginx

#编译并安装

make && make install

#运行nginx
cd //home/work/nginx/sbin
./nginx
```

- 防火墙

```bash
systemctl status firewalld.service
systemctl stop firewalld.service
systemctl disable firewalld.service
systemctl status firewalld.service
```

- Nginx 相关命令

  4.1 版本查看

```bash
nginx  -v
#nginx version: nginx/1.12.2
```

4.2 查看加载的模块

```bash
 nginx -V
#nginx version: nginx/1.12.2
# built by gcc 4.8.5 20150623 (Red Hat 4.8.5-28) (GCC)
# configure arguments: --add-module=/root/nginx-sticky-1.2.5/
```

4.3 启停命令

4.3.1 启动

```bash
 nginx
```

4.3.2 停止

```bash
nginx -s stop
nginx -s quit
```

4.3.3 动态加载

```bash
 nginx -s reload
```

4.3.4 测试配置文件 nginx.conf 正确性

```bash
nginx  -t
# nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
# nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
```

`nginx -s stop`:此方式相当于先查出 nginx 进程 id 再使用 kill 命令强制杀掉进程。

`nginx -s reload`:动态加载，当配置文件 nginx.conf 有变化时执行该命令动态加载。

4.4 开机自启动

编辑/etc/rc.d/rc.local 文件，新增一行/usr/local/nginx/sbin/nginx

```bash
cd /etc/rc.d
sed -i '13a /usr/local/nginx/sbin/nginx' /etc/rc.d/rc.local
chmod u+x rc.local
```
