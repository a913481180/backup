---
title: 嵌入式tools
date: 2021-01-01 10:12:33
categories: 
- 嵌入式
---

# linux下的嵌入式开发工具
## 串口调试
查看串口：`ls -l /dev/tty*`

串口调试工具：`picocom` `putty`
> 使用时需要root权限

#### picocom
查看帮助：`picocom -h `

使用方法如：`picocom -b 115200 /dev/ttyUSB0`

