---
title: Bootloader、Linux内核和文件系统编译
date: 2020-12-08 22:23:45
categories:
- experiments 
---
# 交叉编译固件及系统测试

## 一、目的

掌握虚拟机软件的安装和配置，掌握Ubuntu系统的安装和配置，掌握交叉编译器的安装，掌握Bootloader、Linux内核和文件系统编译方法，并进行硬件平台系统功能测试。

## 二、内容

1、在Windowns环境下安装虚拟机功能软件，在虚拟机环境下安装Ubuntu系统并对系统进行配置。

2、安装ARM交叉编译器，配置环境变量并功能测试。

3、通过对Bootloader、Linux内核和文件系统源码交叉编译，生成系统固件，记录编译结果。

4、调试串口连接硬件平台，对Bootloader和Linux环境下功能命令测试，记录测试结果。

## 三、设备

1、主机电脑。2、ARM实验箱3、调试连接配件

**四、实验步骤**

**4.1** Ubuntu系统的安装和配置

下载Ubuntu系统16.04稳定版系统映像，下载地址：

[**http://releases.ubuntu.com/16.04.6/?\_ga=2.221103960.1120206391.1590239549-144656828.1590239549**](http://releases.ubuntu.com/16.04.6/?_ga=2.221103960.1120206391.1590239549-144656828.1590239549)

在虚拟机中安装Ubuntu系统，根据自己电脑的配置情况，选择CPU数量和内存大小，硬盘分区大小等配置，安装结果下图所示。(实验室提供华清远见开发环境参考，无需安装系统)

在虚拟机中配置网络适配器模式，理解NAT模式和桥接模式的区别。

开机进入Ubuntu系统，进行网络配置，掌握图形界面和命令行下配置网络方法，理解ifconfig命令的使用方法，

命令行方式下，vmware虚拟机网络方式为桥接模式，为ubuntu系统配置一个静态的IP地址。假设使用的网络地址为192.168.100.X段，默认为ubuntu系统分配的IP地址是192.168.100.192 。

如果想修改ubuntu系统的IP地址，可以执行以下命令，修改文件内容。

linux@ubuntu64-vm:~$ sudo vim /etc/network/interfaces

修改完成后，重新启动网络配置。

linux@ubuntu64-vm:~$ sudo /etc/init.d/networking restart

**4. 3 安装交叉编译器并测试**

Ubuntu系统中包含四个版本的交叉编译器，位于路径/usr/local/toolchain目录下，不用解压直接使用即可。

修改bash启动文件，执行以下命令：

linux@ubuntu64-vm:~$ sudo vim /etc/bash.bashrc

在bash.bashrc文件最后添加以下内容并保存退出：

export PATH=$PATH:/usr/local/toolchain/toolchain-4.5.1/bin/

再退出bash终端重新打开，执行ARM交叉编译器命令，获取如下信息说明交叉编译器配置完成。

linux@ubuntu64-vm:~$ arm-none-linux-gnueabi-gcc -v

**4.4 Linux内核编译**

1. 首先在虚拟机工作目录下用个人学号建立自己的工作目录。

linux@ubuntu64-vm:~$ mkdir /home/linux/2018011222/ -p

1. 通过U盘或者共享目录将Linux内核源码压缩包拷贝到该目录里面。

1. 拷贝后进入该目录，解压源码。

linux@ubuntu64-vm:~/2018011222$ tar xvf linux-3.4.39.tar.xz

1. 拷贝ext4文件系统制作工具到/usr/bin目录下。

1. 进入内核源码目录，查看源码结构。
2. 拷贝硬件平台的标准配置文件为.config文件。

$ cp arch/arm/configs/fs6818M4\_defconfig\_v5 .config

1. 执行内核配置命令进入内核配置界面。

$ make menuconfig

查看System type和Deviec Driver配置情况，

1. 保存退出后，编译内核，编译结果如下图。(比较耗时间，15分钟左右，可以先做4.5小节内容)

$ make uImage

1. 执行脚本打包内核镜像，内核镜像文件为boot.img，文件格式是ext4格式，其中包括开机LOGO，ramdisk文件系统和uImage内核镜像，生成boot.img文件，

$./mkboot.sh

**4.5 Linux文件系统镜像制作**

1、通过U盘或者共享目录将文件系统源码压缩包拷贝到/source目录下。

解压文件系统源码压缩包，查看文件系统内容。

$tar xvf rootfs.tar.xz

3、将文件系统打包成ext4格式的镜像，生成system\_linux.img文件

$make\_ext4fs -s system\_linux.img -l 300M rootfs

**4.6 Bootloader**** 环境测试**

1. 连接实验箱5V电源线，连接终

端调试串口线和以太网线到电脑主

机，如右图。

(USB OTA调试线烧写镜像用，

可以不连)

1. 安装USB转串口驱动程序，确

认当前电脑上使用的串口号，设置

电脑上串口调试软件putty。

1. 上电启动实验箱，在终端中可以看到启动信息，快速按空格键进入bootloader环境。
2. 通过print、setenv、saveenv等命令测试bootloader，

**4.7**  **嵌入式**** Linux ****环境测试**

按复位键重启硬件系统，等待平台系统进入Linux环境，测试三条Linux命令，

# 应用程序开发

## 一、目的

掌握NFS挂载Linux启动方法，掌握Ubuntu 环境下交叉编译应用程序的方法。

## 二、内容

1、使用Vim或者Gedit 文本编译器编写C 语言、汇编语言程序和Makefile文件，分别计算公式3\*x2+2\*x3 的值和计算4个输入数的平均数。

2、编写Makefile文件，通过make编译得到ARM 可执行程序formula 和average。

3、配置实验箱启动方式为NFS启动，执行并测试程序。

## 三、设备

1、主机电脑。2、ARM实验箱3、调试连接配件

## 四、原理

掌握嵌入式Linux 应用开发所用到的一些常用工具，包括编辑工具vim、gedit、编译工具gcc、MakeFile 文件及调试工具gdb。

正确掌握C 语言、汇编语言程序和Makefile编写语法和格式。

## 五、步骤

### 5.1 计算公式3\*x^2+2\*x3

1、在Linux 主机上进入自己学号的工作目录，执行如下命令。

$cd /home/linux/2018011222/

$mkdir exam1

$cd exam1

$ gedit formula.c

1. 在formula.c 程序里面输入以下内容。

#include\&lt;stdio.h\&gt;

extern int compute(int data);

int main()

{

int x;

printf(&quot;Input x=&quot;);

scanf(&quot;%d&quot;,&amp;x);

printf(&quot;The result is %d\n&quot;,compute(x));

return 0;

}

3、执行命令生成汇编文件

$ gedit compute.S

包含计算3\*x^2+2\*x^3过程的汇编文件为compute.S内容如下:.text

.align

.global compute

compute:

stmfd sp!,{r4-r11,r14}

mul r1,r0,r0

mov r3,#2

mul r2,r1,r3

mov r3,#3

mul r4,r1,r3

mla r0,r2,r0,r4

ldmfd sp!,{r4-r11,r15}

4、自行编写Makefile 文件，实现对c语言程序和汇编程序的编译得到formula可执行程序。

5、在exam1目录下执行make命令得到可执行文件formula

$make

6、应用strip工具精简可执行程序

$arm-none-linux-gnueabi-strip formula

7、file命令可以查看formula程序属性，截图保存formula程序信息到实验报告中。

$file formula

### 5.2 计算4 个数的平均值

1、在Linux 主机上建立工作目录，执行如下命令。

$cd /home/linux/2018011222/

$mkdir exam2

$cd exam2

$gedit avg.c

1. 在avg.c程序里面输入以下内容。

#include\&lt;stdio.h\&gt;

extern int average(int i, int j,int k,int l);

int main(void)

{

int a,b,c,d,avr;

printf(&quot;Input 4 number: \n&quot;);

scanf(&quot;%d&quot;,&amp;a);

scanf(&quot;%d&quot;,&amp;b);

scanf(&quot;%d&quot;,&amp;c);

scanf(&quot;%d&quot;,&amp;d);

printf(&quot;a=%d,b=%d,c=%d,d=%d\n&quot;,a,b,c,d);

avr= average(a,b,c,d);

printf(&quot;(%d+,%d+,%d+,%d)/4=,%d\n&quot;,a,b,c,d,avr);

return 0;

}

3、执行命令生成汇编文件

$gedit average.S

自行编写计算平均值的汇编文件average.S程序。

4、自行编写Makefile 文件，实现对c语言程序和汇编程序的编译得到formula可执行程序。

5、在exam2目录下执行make命令得到可执行文件average

$make

6、应用strip工具精简可执行程序

$ arm-none-linux-gnueabi-strip average

7、file命令可以查看average程序属性，

$file average

### 5.3配置NFS启动方式

1、设置好电脑主机的IP地址为192.168.100.192 ，配置好主机的NFS服务，默认是启动的。

2、在Bootloader环境下配置启动参数， **注意格式不要出错！！！**

```
FS6818# setenv serverip 192.168.100.192

FS6818#setenv ipaddr 192.168.100.100

FS6818# setenv gatewayip 192.168.100.1

FS6818# setenv bootcmd ext4load mmc 2:1 0x48000000 uImage\;bootm 0x48000000

FS6818# setenv bootargs root=/dev/nfs nfsroot=192.168.100.192:/source/rootfs console=ttySAC0,115200 init=/linuxrc ip=192.168.100.100:::::eth0:off

FS6818# saveenv
```

保存后复位ARM实验箱，等待进入NFS启动的文件系统，如下图。

![](RackMultipart20210826-4-1d1i54l_html_ea6ac7d22326c54c.png)

拷贝之前编译好的可执行文件formula和average到主机NFS共享目录下wsn6818bp\_test，这样在putty终端中就可以运行测试结果，

![](RackMultipart20210826-4-1d1i54l_html_458a03b2d32152d4.png)
