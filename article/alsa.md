---
title: Alsa库移植和安装
date: 2019-11-22 22:11:33
categories:
- 嵌入式
---

### alsa库下载  
`ftp://ftp.alsa-project.org/pub`

### 编译安装  
解压进入文件夹  
`./configure  --prefix=/home/mine/alsa	//生成文件存放的位置`  
开始安装驱动
`make && sudo make install `

### 交叉编译  

使用命令

`./configure --host=arm-jyxtec-linux-gnueabi --prefix=/usr/share/arm-alsa`

>--host参数指定了交叉编译器为“arm-jyxtec-linux-gnueabi（根据自己板子上的工具链稳准）"，因此必须确保交叉编译器已成功配置（也就是已经导出为全局环境变量，切记！切记！我就在这浪费了很多时间，我的习惯是添加据对路径），且可以在shell里直接调用；--prefix参数指定了alsa-lib的目标安装目录，之后的alsa-util配置也一样

使用命令

`./configure --host=arm-jyxtec-linux-gnueabi --prefix=/usr/share/arm-alsa --with-alsa-inc-prefix=/usr/share/arm-alsa/include --with-alsa-prefix=/usr/share/arm-alsa/lib --disable-alsamixer --disable-xmlto`

>--with-alsa-inc-prefix和--with-alsa-prefix分别指定了交叉编译util应用所需要的alsa-lib的头文件和库文件；--disable-alsamixer表示不编译生成alsamixer这个应用程序，因为该程序依赖于ncurses这个图形界面库，目前我们对于该库还不能交叉编译，故放弃

### 可能出现的错误  
---

` error: this packages requires a curses library`  

**解决办法：**  

`apt-get install libncurses5-dev`


--- 

` error: panelw library not found`  

**解决办法：**
```
sudo ln -s libpanelw.so.5 /usr/lib/libpanelw.so
sudo ln -s libformw.so.5 /usr/lib/libformw.so
sudo ln -s libmenuw.so.5 /usr/lib/libmenuw.so
sudo ln -s libncursesw.so.5 /lib/libncursesw.so
```
--- 

**解决办法：**

根据自己的错误提示来输入命令  
```
如果提示是t-ru.gmo的话，就用命令：touch alsaconf/po/t-ru.gmo
如果提示是t-ja.gmo的话，就用命令：touch alsaconf/po/t-ja.gmo
```
