---
title: 调用百度语音识别api
date: 2019-11-28 19:22:34
categories:
  - 嵌入式
---

## 调用百度语音识别 api

### 注册账号申请 api key

### 下载

- 源码：语音识别、语音合成

`git clone https://github.com/Baidu-AIP/speech-demo`

- 依赖(curl.h)：

`apt install libcurl4-openssl-dev`

### 编辑源文件

编辑`asrmain.c` `ttsmain.c`填入自己的 keys，修改相应参数

编译`sh build_xxxxxxx`

## 语音唤醒

### snowboy 下载

`git clone https://github.com/Kitt-AI/snowboy.git`

### 依赖

`sudo apt-get -y install libasound2-dev`  
`sudo apt-get install python3-pyaudio`
`sudo apt-get install swig3.0 sox`  
`sudo apt-get install libatlas-base-dev`

通过 ffplay 工具(安装 ffmpeg 会带上这个工具)。

`ffplay -ar 44100 -channels 2 -f s16le -i 123.pcm`
