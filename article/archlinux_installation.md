---
title: 安装archlinux系统
date: 2020-11-22 20:22:22
categories:
- linux
---
# archlinux 安装过程
##  连接wifi

`ip link //查看wifi设备`

`ip link set 设备 up //启用设备`

`iwlist 设备 scan | grep ESSID //扫描查看wifi名称`

或者使用

`iw dev scan ....`

`wifi-menu`
### 连接wpa加密wifi

`wpa_passphrase wifi名称 密码 > internet.conf`

`wpa_supplicant -i 网卡 -c internet.conf &`

`dhcpcd 网卡 &`


## 检查是否联通
(ctrl+c停止)

`ping www.baidu.com`

## 更新系统时钟
`timedatectl set-ntp true`

## 查看当前的可用磁盘

`fdisk -l`
## 创建新分区

`fdisk /dev/sda    //注意磁盘名称`

--\>n -->回车 -->回车 -->回车 -->+500m  （boot分区）
 
--\>n -->回车 -->回车 -->回车 -->+6G      （根分区）

--\>n -->回车 -->回车 -->回车 -->回车  （将剩余空间全部分配给该分区）
 
--\>w    （写入）

## 根目录格式化为ext4

请注意自己的对应的目录是那块区域
```
	mkfs.fat -F32 /dev/sda1     /// boot分区
	mkswap /dev/sda2   //swap 分区
	mkfs.ext4 /dev/sda3
```
## 打开swap分区#
请注意自己的 swap分区是哪块

`swapon /dev/sda2`

## 更改pacman配置文件

`vim /etc/pacman.conf`

去掉Color的注释,用于安装时提示信息

## 挂载分区#
```
	mount  /dev/sda3 /mnt     //主分区内
	ls /mnt  /////查看是否挂载成功，若出现lost+found 则代表挂载成功
	mkdir /mnt/boot
	mount /dev/sda1 /mnt/boot      /// boot分区
```
## 配置源#
将国内源放到第一位 

`vim /etc/pacman.d/mirrorlist`

## 刷新源#

` pacman -Syy`

## 安装基本包

`pacstrap /mnt linux linux-firmware base base-devel`

## 生成fstab文件

`genfstab -U /mnt >> /mnt/etc/fstab`

## 切换环境

`arch-chroot /mnt`

## 配置时区

`ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`

## 硬件时间

`hwclock --systohc`

## 安装vim

`pacman -S vim`

## 退出

`exit`

## 设置语言
//将/etc/locale.gen中en_US.UTF-8的注释去掉,避免中文乱码,推荐先使用英文//

`vim /mnt/etc/locale.gen`

### 切换环境
```
	arch-chroot /mnt
	locale-gen
	exit
```
## 配置 /etc/locale.conf文件

`vim /mnt/etc/locale.conf`

写入

`LANG=en_US.UTF-8`

### 切换环境

`arch-chroot /mnt`

### 重置root密码

`passwd`

## 安装网络相关的包#

` pacman -S iw wpa_supplicant dialog net-tools networkmanager dhcpcd netctl`

## 安装编码

`pacman -S intel-ucode（amd-ucode） os-prober(寻找其他系统的工具） efibootmgr（efi启动）`

## 安装引导
```
	pacman -S grub
	mkdir /boot/grub
	grub-mkconfig -o /boot/grub/grub.cfg
	grub-install --target=i386-pc /dev/sda (整个磁盘） //// efi:grub-install --target=x86_64-efi --efi-directory=/boot
```
### 退出当前环境#
` exit`
## 关闭网络

`killall wpa_supplicant dhcpcd`

## 重启
`reboot`
## 拔u盘

## 安装软件
- 图形界面：`xorg` `xorg-server` `xorg-xinit`
- 男人：`man` 
+ 文件管理器：`ranger`
+ 电脑参数：`neofetch`
### 设置
xrandr
xrandr --output screen --mode 1280x800
feh --randomize --bg-fill /地址
picom


