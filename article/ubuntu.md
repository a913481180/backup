---
title: ubuntu系统安装
date: 2020-12-22 23:33:33
categores:
- linux
--- 

# ubuntu系统安装
## 安装
### 下载镜像
### 软碟通写入硬盘镜像
### 重启进入安装界面

|分区：||
|-|-|
|/boot/efi/|引导分区|
|/boot|启动|
|/|根目录|
|swap|交换分区|
|/home|用户存放文件|
## 配置
* 设置根用户密码：
`sudo passwd`
* 换源
`sudo vim /etc/apt/sources.list`
粘贴
```
#添加阿里源
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
#添加清华源,默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse multiverse
```
* 执行更新命令：
`sudo apt-get update`
`sudo apt-get upgrade`


## 安装软件 

* vim
* neofetch
* ranger
* google chrome
* 搜狗输入法
* 网易云、QQ音乐
* wps
* 字体安装
windows的字体一般存放在c:/windows/fonts目录下，我拷贝到linux下的字体有：
`simfang.ttf` 仿宋体
`simhei.ttf `黑体
`simkai.ttf `楷体

宋体：`simsunb.ttf `和`simsun.ttc`
`simsun.ttf `宋体和新宋体，原文件名`simsun.ttc`
微软雅黑：`msyhbd.ttf`

下载字体文件，然后复制到Linux系统中的/usr/share/fonts文件夹中。
修改权限，并更新字体缓存
`sudo chmod -R 777  /usr/share/fonts/windows-font`
`cd /usr/share/fonts/windows-font`

执行以下命令,生成字体的索引信息：
`sudo mkfontscale`
`sudo mkfontdir`
运行fc-cache命令更新字体缓存。
`sudo fc-cache -fv`



