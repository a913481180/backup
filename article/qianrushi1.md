---
title: Bootloader、Linux内核和文件系统编译
date: 2019-12-08 22:23:45
categories:
  - experiments
---

# 交叉编译固件及系统测试

## 一、目的

掌握虚拟机软件的安装和配置，掌握 Ubuntu 系统的安装和配置，掌握交叉编译器的安装，掌握 Bootloader、Linux 内核和文件系统编译方法，并进行硬件平台系统功能测试。

## 二、内容

1、在 Windowns 环境下安装虚拟机功能软件，在虚拟机环境下安装 Ubuntu 系统并对系统进行配置。

2、安装 ARM 交叉编译器，配置环境变量并功能测试。

3、通过对 Bootloader、Linux 内核和文件系统源码交叉编译，生成系统固件，记录编译结果。

4、调试串口连接硬件平台，对 Bootloader 和 Linux 环境下功能命令测试，记录测试结果。

## 三、设备

1、主机电脑。2、ARM 实验箱 3、调试连接配件

**四、实验步骤**

**4.1** Ubuntu 系统的安装和配置

下载 Ubuntu 系统 16.04 稳定版系统映像，下载地址：

[http://releases.ubuntu.com/16.04.6/?\_ga=2.221103960.1120206391.1590239549-144656828.1590239549](http://releases.ubuntu.com/16.04.6/?_ga=2.221103960.1120206391.1590239549-144656828.1590239549)

在虚拟机中安装 Ubuntu 系统，根据自己电脑的配置情况，选择 CPU 数量和内存大小，硬盘分区大小等配置，安装结果下图所示。(实验室提供华清远见开发环境参考，无需安装系统)

在虚拟机中配置网络适配器模式，理解 NAT 模式和桥接模式的区别。

开机进入 Ubuntu 系统，进行网络配置，掌握图形界面和命令行下配置网络方法，理解 ifconfig 命令的使用方法，

命令行方式下，vmware 虚拟机网络方式为桥接模式，为 ubuntu 系统配置一个静态的 IP 地址。假设使用的网络地址为 192.168.100.X 段，默认为 ubuntu 系统分配的 IP 地址是 192.168.100.192 。

如果想修改 ubuntu 系统的 IP 地址，可以执行以下命令，修改文件内容。

```bash
linux@ubuntu64-vm:~$ sudo vim /etc/network/interfaces
```

修改完成后，重新启动网络配置。

```bash
linux@ubuntu64-vm:~$ sudo /etc/init.d/networking restart
```

**4. 3 安装交叉编译器并测试**

Ubuntu 系统中包含四个版本的交叉编译器，位于路径/usr/local/toolchain 目录下，不用解压直接使用即可。

修改 bash 启动文件，执行以下命令：

```bash
linux@ubuntu64-vm:~$ sudo vim /etc/bash.bashrc
```

在 bash.bashrc 文件最后添加以下内容并保存退出：

```bash
export PATH=$PATH:/usr/local/toolchain/toolchain-4.5.1/bin/
```

再退出 bash 终端重新打开，执行 ARM 交叉编译器命令，获取如下信息说明交叉编译器配置完成。

```bash
linux@ubuntu64-vm:~$ arm-none-linux-gnueabi-gcc -v
```

**4.4 Linux 内核编译**

1. 首先在虚拟机工作目录下用个人学号建立自己的工作目录。

```bash
linux@ubuntu64-vm:~$ mkdir /home/linux/2018011222/ -p
```

1. 通过 U 盘或者共享目录将 Linux 内核源码压缩包拷贝到该目录里面。

1. 拷贝后进入该目录，解压源码。

```bash
linux@ubuntu64-vm:~/2018011222$ tar xvf linux-3.4.39.tar.xz
```

1. 拷贝 ext4 文件系统制作工具到/usr/bin 目录下。

1. 进入内核源码目录，查看源码结构。
1. 拷贝硬件平台的标准配置文件为.config 文件。

```bash
$ cp arch/arm/configs/fs6818M4_defconfig_v5 .config
```

1. 执行内核配置命令进入内核配置界面。

```bash
$ make menuconfig
```

查看 System type 和 Deviec Driver 配置情况，

1. 保存退出后，编译内核，编译结果如下图。(比较耗时间，15 分钟左右，可以先做 4.5 小节内容)

```bash
$ make uImage
```

1. 执行脚本打包内核镜像，内核镜像文件为 boot.img，文件格式是 ext4 格式，其中包括开机 LOGO，ramdisk 文件系统和 uImage 内核镜像，生成 boot.img 文件，

```bash
$./mkboot.sh
```

**4.5 Linux 文件系统镜像制作**

1、通过 U 盘或者共享目录将文件系统源码压缩包拷贝到/source 目录下。

解压文件系统源码压缩包，查看文件系统内容。

```bash
$tar xvf rootfs.tar.xz
```

3、将文件系统打包成 ext4 格式的镜像，生成 system_linux.img 文件

```bash
$make_ext4fs -s system_linux.img -l 300M rootfs
```

**4.6 Bootloader\*\*** 环境测试\*\*

1. 连接实验箱 5V 电源线，连接终

端调试串口线和以太网线到电脑主

机，如右图。

(USB OTA 调试线烧写镜像用，

可以不连)

1. 安装 USB 转串口驱动程序，确

认当前电脑上使用的串口号，设置

电脑上串口调试软件 putty。

1. 上电启动实验箱，在终端中可以看到启动信息，快速按空格键进入 bootloader 环境。
2. 通过 print、setenv、saveenv 等命令测试 bootloader，

**4.7** **嵌入式\*\*** Linux \***\*环境测试**

按复位键重启硬件系统，等待平台系统进入 Linux 环境，测试三条 Linux 命令，

# 应用程序开发

## 一、目的

掌握 NFS 挂载 Linux 启动方法，掌握 Ubuntu 环境下交叉编译应用程序的方法。

## 二、内容

1、使用 Vim 或者 Gedit 文本编译器编写 C 语言、汇编语言程序和 Makefile 文件，分别计算公式 3\*x2+2\*x3 的值和计算 4 个输入数的平均数。

2、编写 Makefile 文件，通过 make 编译得到 ARM 可执行程序 formula 和 average。

3、配置实验箱启动方式为 NFS 启动，执行并测试程序。

## 三、设备

1、主机电脑。2、ARM 实验箱 3、调试连接配件

## 四、原理

掌握嵌入式 Linux 应用开发所用到的一些常用工具，包括编辑工具 vim、gedit、编译工具 gcc、MakeFile 文件及调试工具 gdb。

正确掌握 C 语言、汇编语言程序和 Makefile 编写语法和格式。

## 五、步骤

### 5.1 计算公式 3\*x^2+2\*x3

1、在 Linux 主机上进入自己学号的工作目录，执行如下命令。

```bash
$cd /home/linux/2018011222/

$mkdir exam1

$cd exam1

$ gedit formula.c
```

1. 在 formula.c 程序里面输入以下内容。

```c
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
```

3、执行命令生成汇编文件

```bash
$ gedit compute.S
```

包含计算 3\*x^2+2\*x^3 过程的汇编文件为 compute.S 内容如下:

```S
.text

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
```

4、自行编写 Makefile 文件，实现对 c 语言程序和汇编程序的编译得到 formula 可执行程序。

5、在 exam1 目录下执行 make 命令得到可执行文件 formula

```bash
$make
```

6、应用 strip 工具精简可执行程序

```bash
$arm-none-linux-gnueabi-strip formula
```

7、file 命令可以查看 formula 程序属性，截图保存 formula 程序信息到实验报告中。

```bash
$file formula
```

### 5.2 计算 4 个数的平均值

1、在 Linux 主机上建立工作目录，执行如下命令。

```bash
$cd /home/linux/2018011222/


$mkdir exam2

$cd exam2

$gedit avg.c
```

1. 在 avg.c 程序里面输入以下内容。

```c
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
```

3、执行命令生成汇编文件

```bash
$gedit average.S
```

自行编写计算平均值的汇编文件 average.S 程序。

4、自行编写 Makefile 文件，实现对 c 语言程序和汇编程序的编译得到 formula 可执行程序。

5、在 exam2 目录下执行 make 命令得到可执行文件 average

```bash
$make
```

6、应用 strip 工具精简可执行程序

$ arm-none-linux-gnueabi-strip average

7、file 命令可以查看 average 程序属性，

```bash
$file average
```

### 5.3 配置 NFS 启动方式

1、设置好电脑主机的 IP 地址为 192.168.100.192 ，配置好主机的 NFS 服务，默认是启动的。

2、在 Bootloader 环境下配置启动参数， **注意格式不要出错！！！**

```log
FS6818# setenv serverip 192.168.100.192

FS6818#setenv ipaddr 192.168.100.100

FS6818# setenv gatewayip 192.168.100.1

FS6818# setenv bootcmd ext4load mmc 2:1 0x48000000 uImage\;bootm 0x48000000

FS6818# setenv bootargs root=/dev/nfs nfsroot=192.168.100.192:/source/rootfs console=ttySAC0,115200 init=/linuxrc ip=192.168.100.100:::::eth0:off

FS6818# saveenv
```

保存后复位 ARM 实验箱，等待进入 NFS 启动的文件系统，如下图。

![](RackMultipart20210826-4-1d1i54l_html_ea6ac7d22326c54c.png)

拷贝之前编译好的可执行文件 formula 和 average 到主机 NFS 共享目录下 wsn6818bp_test，这样在 putty 终端中就可以运行测试结果，

![](RackMultipart20210826-4-1d1i54l_html_458a03b2d32152d4.png)
