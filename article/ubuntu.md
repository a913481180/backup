---
title: ubuntu系统安装
date: 2020-12-22 23:33:33
categores:
  - linux
tags:
- system
---


## 安装

### 下载镜像

### 软碟通写入硬盘镜像

### 重启进入安装界面

| 分区：     |                                                                                      |
| ---------- | ------------------------------------------------------------------------------------ |
| /boot/efi/ | 必须，引导分区，256mb-512mb                                                          |
| /boot      | 非必须，引导分区，ext4格式，256mb                                                                         |
| swap       | 非必须，交换分区，虚拟内存，swap格式。内存小于 2g->2g，内存小于 8g->4g-8g，内存大于 8g->8g-16g，内存大于 16->16g |
| /          | 必须，主分区，用于安装系统和软件，ext4格式 ，30gb                                                                       |
| /home      | 用户存放文件，ext4格式                                                                         |

>`/bin` `/sbin`  `/lib` `/etc` `/dev`这五个目录不要与根目录`/`所在分区分开。否则系统会不能正常引导

## 配置

- 设置根用户密码：
  `sudo passwd`
- 换源
  `sudo vim /etc/apt/sources.list`
  粘贴

ubuntu

```txt
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

debain

```txt
#阿里
deb https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb https://mirrors.aliyun.com/debian-security/ bullseye-security main
deb-src https://mirrors.aliyun.com/debian-security/ bullseye-security main
deb https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
#中科大
deb https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
deb https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
deb https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main contrib non-free
#清华
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
#华为
deb https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
deb https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
deb-src https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
deb https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
#腾讯
deb https://mirrors.tencent.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.tencent.com/debian/ bullseye main non-free contrib
deb https://mirrors.tencent.com/debian-security/ bullseye-security main
deb-src https://mirrors.tencent.com/debian-security/ bullseye-security main
deb https://mirrors.tencent.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.tencent.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.tencent.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.tencent.com/debian/ bullseye-backports main non-free contrib
#网易
deb https://mirrors.163.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.163.com/debian/ bullseye main non-free contrib
deb https://mirrors.163.com/debian-security/ bullseye-security main
deb-src https://mirrors.163.com/debian-security/ bullseye-security main
deb https://mirrors.163.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.163.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.163.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.163.com/debian/ bullseye-backports main non-free contrib
```

- 执行更新命令：
  `sudo apt-get update`
  `sudo apt-get upgrade`

## 安装软件

- vim
- neofetch
- ranger
- rar解压软件
  - (<https://www.rarlab.com/download.htm>) 下载最新版本的linux版本rar软件
  - 解压后进入文件夹，执行`sudo make`命令
  - 解压：`rar x test.rar`
  - 解压到当前目录：`rar e test.rar`
  - 压缩：`rar a test.rar /test`
  - 也可以使用`unrar`命令提取rar压缩文件
- 截图工具：`flameshot`
  - 安装：`sudo apt install flameshot`
  - 使用：`flameshot gui`
  - 添加快捷键：`/usr/bin/flameshot gui`
- 远程连接windows软件:`rdesktop`
  - 安装：`sudo apt install rdesktop`
  - 使用：`rdesktop xxx.xxx.xxx.xxx -u kk -p 123456`
  - 参数：
    - `-f`：全屏模式ctrl+alt+enter可切换全屏
    - `-a 16`： 16位色彩
    - `-z`：启用RDP数据流压缩
    - `-x lan`：使用局域网级别的图像质量
    - `-r clipboard`：运行共享剪切板
    - `-g 1920x1080+0+0`：分辨率,窗口位置
    - `-r sound:local`：同步声音
    - `-r disk:share=/home`：win10网上邻居虚拟一个映射盘，ubuntu挂载到远程主机上
- nodejs
  - 拷贝解压

```bash
 VERSION=v10.15.0
 DISTRO=linux-x64
 sudo mkdir -p /usr/local/lib/nodejs
 sudo tar -xJvf node-$VERSION-$DISTRO.tar.xz -C /usr/local/lib/nodejs 
```

- 环境变量`~/.profile`：`export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH`
- 刷新：`source ~/.profile`
- google chrome
`https://www.google.cn/intl/zh-CN/chrome/`
- 搜狗输入法(ubuntu20移除qt4,会出现死机情况)
`https://shurufa.sogou.com/`
- 百度输入法
- 谷歌输入法(推荐)
`sudo apt install fcitx fcitx-googlepinyin`
- 网易云、QQ 音乐
- vscode
- vitrulbox
- unzip
- gzip
- mpv、vlc
- 百度网盘
- 有道翻译
- 迅雷
- wps
- wps字体安装
  windows 的字体一般存放在 c:/windows/fonts 目录下，我拷贝到 linux 下的字体有：
  `simfang.ttf` 仿宋体
  `simhei.ttf`黑体
  `simkai.ttf`楷体

宋体：`simsunb.ttf`和`simsun.ttc`
`simsun.ttf`宋体和新宋体，原文件名`simsun.ttc`
微软雅黑：`msyhbd.ttf`

下载字体文件，然后复制到 Linux 系统中的/usr/share/fonts/win-font 文件夹中。
修改权限，并更新字体缓存
`sudo chmod -R 777  /usr/share/fonts/win-font`
`cd /usr/share/fonts/win-font`

执行以下命令,生成字体的索引信息：
`sudo mkfontscale`
`sudo mkfontdir`
运行 fc-cache 命令更新字体缓存。
`sudo fc-cache -fv`
