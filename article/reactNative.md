---
title: react Native
date: 2021-01-09 20:00:00
categories: 
- web 
tags:
- app
---
# React

## 环境搭建

- node.js python2(不能python3) jdk android_studio
- 配置环境变量


## 创建项目

- 使用React Native内建的命令工具进行创建，也可以使用node自带的npx命令来安装
`npx react-native init test_project`
-准备 Android 设备
你需要准备一台 Android 设备来运行 React Native Android 应用。这里所指的设备既可以是真机，也可以是模拟器。后面我们所有的文档除非特别说明，并不区分真机或者模拟器。Android 官方提供了名为 Android Virtual Device（简称 AVD）的模拟器。此外还有很多第三方提供的模拟器如Genymotion、BlueStack 等。一般来说官方模拟器免费、功能完整，但性能较差。第三方模拟器性能较好，但可能需要付费，或带有广告。

使用 Android 真机
你也可以使用 Android 真机来代替模拟器进行开发，只需用 usb 数据线连接到电脑，然后遵照在设备上运行这篇文档的说明操作即可。

使用 Android 模拟器
你可以使用 Android Studio 打开项目下的"android"目录，然后可以使用"AVD Manager"来查看可用的虚拟设备,如果你刚刚才安装 Android Studio，那么可能需要先创建一个虚拟设备。点击"Create Virtual Device..."，然后选择所需的设备类型并点击"Next"，然后选择S API Level 31 image.
- 编译并运行 React Native 应用
确保你先运行了模拟器或者连接了真机，然后在你的项目目录中运行`yarn android`或者`yarn react-native run-android`。此命令会对项目的原生部分进行编译，同时在另外一个命令行中启动Metro服务对 js 代码进行实时打包处理（类似 webpack）。Metro服务也可以使用yarn start命令单独启动。

如果配置没有问题，你应该可以看到应用自动安装到设备上并开始运行。注意第一次运行时需要下载大量编译依赖，耗时可能数十分钟。此过程严重依赖稳定的代理软件，否则将频繁遭遇链接超时和断开，导致无法运行。

也可以尝试阿里云提供的maven 镜像，将android/build.gradle中的jcenter()和google()分别替换为maven { url 'https://maven.aliyun.com/repository/jcenter' }和maven { url 'https://maven.aliyun.com/repository/google' }（注意有多处需要替换）。

## error
### Could not download react-android-0.71.3-debug.aar
根据报错提供的地址，复制的下载链接到浏览器下载，放到如下位置（前提是你没有改过依赖的存放路径）：
C:\Users\你的用户名\/.gradle\caches\modules-2\files-2.1，接下来在哪个文件夹就因你缺失的文件的不同而不同了，我的是如下路径：
C:\Users\Duan\/.gradle\caches\modules-2\files-2.1\com.facebook.react\react-android\0.71.3
但是一般都能在报错信息中找到，其中有多个文件夹，把刚刚下好的文件放到其中一个含有.pom文件的文件夹里就对了然后重新运行就好了！！！

### “Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0”
根据提示，用如下命令

gradlew --warning-mode all

进行编译,提示如下：
```txt
> Configure project :
The compile configuration has been deprecated for dependency declaration. This will fail with an error in Gradle 7.0. Please use the implementation configuration instead.
```
很显然，在Gradle 7.0，不支持compile了，所以，在build.gradle文件中，将compile改为implementation，问题就解决了。在build.gradle文件中，将compile改为implementation

