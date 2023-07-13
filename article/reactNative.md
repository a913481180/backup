---
title: react Native
date: 2021-01-09 20:00:00
categories: 
- web 
tags:
- app
---
# React Native 0.71

## 环境搭建

### Node

注意 Node 的版本应大于等于 `16`，安装完 Node 后建议设置 npm 镜像（淘宝源）以加速后面的过程（或使用科学上网工具）
注意：不要使用 `cnpm`！cnpm 安装的模块路径比较奇怪，packager 不能正常识别！

### JDK

>低于 0.67 版本的 React Native 需要 JDK 1.8 版本（官方也称 8 版本）。
React Native 需要 `Java Development Kit [JDK] 11`。你可以在命令行中输入 `javac -version`（请注意是 javac，不是 java）来查看你当前安装的 JDK 版本。

### Android Studio

#### 首先下载和安装 Android Studio

国内用户可能无法打开官方链接，请自行使用搜索引擎搜索可用的下载链接。安装界面中选择"Custom"选项，确保选中了以下几项：

- `Android SDK`
- `Android SDK Platform`
- `Android Virtual Device`
然后点击"Next"来安装选中的组件。

#### 安装 Android SDK

Android Studio 默认会安装最新版本的 Android SDK。目前编译 React Native 应用需要的是`Android 13 (Tiramisu)`版本的 SDK（注意 SDK 版本不等于终端系统版本，RN 目前支持 android 5 以上设备）。你可以在 Android Studio 的 SDK Manager 中选择安装各版本的 SDK。

你可以在 Android Studio 的欢迎界面中找到 SDK Manager。点击"Configure"，然后就能看到"SDK Manager"。

SDK Manager 还可以在 Android Studio 的"Preferences"菜单中找到。具体路径是`Appearance & Behavior → System Settings → Android SDK`。

在 SDK Manager 中选择"SDK Platforms"选项卡，然后在右下角勾选"Show Package Details"。展开Android 13 (Tiramisu)选项，确保勾选了下面这些组件（重申你必须使用稳定的代理软件，否则可能都看不到这个界面）：

- `Android SDK Platform 33`
- `Intel x86 Atom_64 System Image`,`Google APIs Intel x86 Atom System Image`（官方模拟器镜像文件，使用非官方模拟器不需要安装此组件）

然后点击"SDK Tools"选项卡，同样勾中右下角的"Show Package Details"。展开"`Android SDK Build-Tools`"选项，确保选中了 React Native 所必须的`33.0.0`版本。你可以同时安装多个其他版本。

最后点击"Apply"来下载和安装这些组件。

#### 配置环境变量

打开`控制面板` -> `系统和安全` -> `系统` -> `高级系统设置` -> `高级` -> `环境变量` -> `新建`，创建一个名为`ANDROID_HOME`的环境变量（系统或用户变量均可），指向你的 Android SDK 所在的目录（具体的路径可能和下图不一致，请自行确认）
SDK 默认是安装在下面的目录：`C:\Users\你的用户名\AppData\Local\Android\Sdk`

你可以在 Android Studio 的"Preferences"菜单中查看 SDK 的真实路径，具体是`Appearance & Behavior` → `System Settings` → `Android SDK。`

你需要关闭现有的命令符提示窗口然后重新打开，这样新的环境变量才能生效。

把一些工具目录添加到环境变量 Path
打开`控制面板` -> `系统和安全` -> `系统` -> `高级系统设置` -> `高级` -> `环境变量`，选中`Path`变量，然后点击编辑。点击新建然后把这些工具目录路径添加进去：platform-tools、emulator、tools、tools/bin

```txt
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\emulator
%ANDROID_HOME%\tools
%ANDROID_HOME%\tools\bin
```

## 创建项目

>如果你之前全局安装过旧的react-native-cli命令行工具，请使用npm uninstall -g react-native-cli卸载掉它以避免一些冲突：
>`npm uninstall -g react-native-cli @react-native-community/cli`

### 使用React Native内建的命令工具进行创建，也可以使用node自带的npx命令来安装

>请不要在目录、文件名中使用中文、空格等特殊符号。请不要单独使用常见的关键字作为项目名（如 class, native, new, package 等等）。请不要使用与核心模块同名的项目名（如 react, react-native 等）。
>请不要在某些权限敏感的目录例如 System32 目录中 init 项目！会有各种权限限制导致不能运行！
>请不要使用一些移植的终端环境，例如git bash或mingw等等，这些在 windows 下可能导致找不到环境变量。请使用系统自带的命令行（CMD 或 powershell）运行。
`npx react-native@latest init test_project`
或指定版本
`npx react-native@X.XX.X init AwesomeProject --version X.XX.X`

### 准备 Android 设备

使用 Android 真机
你也可以使用 Android 真机来代替模拟器进行开发，只需用 usb 数据线连接到电脑，然后遵照在设备上运行这篇文档的说明操作即可。

使用 Android 模拟器
你可以使用 Android Studio 打开项目下的"android"目录，然后可以使用"AVD Manager"来查看可用的虚拟设备,如果你刚刚才安装 Android Studio，那么可能需要先创建一个虚拟设备。点击"Create Virtual Device..."，然后选择所需的设备类型并点击"Next"，然后选择Tiramisu API Level 33 image.

使用第三方模拟器
Genymotion
官方下载地址：<https://www.genymotion.com/download/>
提示：如果你的电脑上已经单独安装了VirtualBox虚拟机软件，可以只下载35MB的Genymotion。
在下载完成之后,在Genymotion面板就出现了你刚刚下载的模拟器,但是我们选择还不能启动,还有关键一步没有设置,就是在Genymotion软件中设置Android SDK的路径--------点击  Settings--ADB--Use custom Android SDK tools(自定义AndroidSDK工具)--Briwse  ,,然后选择你的AndroidSDK的安装路径,如果路径没有问题,在下面就会出现Android SDK tools found successfully的提示,然后就可以了,

夜神模拟器
`adb connect 127.0.0.1:62001`
>启动 react native 项目，android studio编辑器里的模拟器可以正常启动，传输至夜神模拟器报错
检查Android SDK的adb版本和夜神模拟器的adb版本是否一致
使用命令 `adb version` 查看 Android SDK 的adb版本
使用 `Nox_adb version` 查看夜神模拟器的adb版本（需进入夜神模拟器安装目录/bin文件夹下）

解决方案

找到Android SDK的adb文件路径：{安装Path}\android-sdk\platform-tools
`adb.exe`
`AdbWinApi.dll`
`AdbWinUsbApi.dll`
将上面三个Android SDK的adb文件替换夜神模拟器的adb文件(夜神模拟器adb文件路径：{安装Path}\Nox\bin)
删除夜神模拟器bin路径下的`nox_adb.exe`文件，再复制一份Android SDK的`adb.exe`文件到夜神模拟器bin路径下并重命名为`nox_adb.exe`

替换完成后，再次使用命令 adb version 和 Nox_adb version 查看Android SDK和夜神模拟器的adb版本是否一致。
若一致，重新打开模拟器使用命令adb connect 127.0.0.1:62001连接模拟器即可

- 编译并运行 React Native 应用

确保你先运行了模拟器或者连接了真机，然后在你的项目目录中运行`yarn android`或者`yarn react-native run-android`。

```bash
cd AwesomeProject
yarn android
# 或者
yarn react-native run-android
```

此命令会对项目的原生部分进行编译，同时在另外一个命令行中启动Metro服务对 js 代码进行实时打包处理（类似 webpack）。Metro服务也可以使用`yarn start`命令单独启动。

如果配置没有问题，你应该可以看到应用自动安装到设备上并开始运行。注意第一次运行时需要下载大量编译依赖，耗时可能数十分钟。此过程严重依赖稳定的代理软件，否则将频繁遭遇链接超时和断开，导致无法运行。

也可以尝试阿里云提供的maven 镜像，将`android/build.gradle`中加入

```gradle
 repositories {
        maven { url 'https://repo1.maven.org/maven2' }
        maven { url 'https://plugins.gradle.org/m2/' }
        maven { url 'https://maven.aliyun.com/repository/jcenter' }
        maven { url 'https://maven.aliyun.com/repository/google' }
        maven { url 'https://maven.aliyun.com/repository/public' }
        maven { url 'https://maven.aliyun.com/repository/mapr-public' }
        mavenCentral()
        google()
    }
```

`npx react-native run-android`只是运行应用的方式之一。你也可以在 Android Studio 中直接运行应用。

译注：建议在run-android成功后再尝试使用 Android Studio 启动。请不要轻易点击 Android Studio 中可能弹出的建议更新项目中某依赖项的建议，否则可能导致无法运行。

如果你无法正常运行，遇到奇奇怪怪的红屏错误，先回头仔细对照文档检查，然后可以看看问题讨论区。不同时期不同版本可能会碰到不同的问题，我们会在论坛中及时解答更新。但请注意千万不要执行 bundle 命令，那样会导致代码完全无法刷新。

### 使用 TypeScript 开始新项目

如果您要开始一个新项目，则有几种不同的上手方法。 您可以使用TypeScript 模板:

`npx react-native init MyApp --template react-native-template-typescript`

如果以上命令失败，则可能是您的 PC 上全局安装了旧版本的`react-native`或`react-native-cli`。 尝试卸载 cli 并使用npx运行 cli.

您可以使用具有两个 TypeScript 模板的Expo:

```bash
npm install -g expo-cli
expo init MyTSProject
```

或者，您可以使用Ignite，它也具有 TypeScript 模板:

```bash
npm install -g ignite-cli
ignite new MyTSProject
```

在已有的项目中添加 TypeScript
将 TypeScript 以及 React Native 和 Jest 的依赖添加到您的项目中。
`yarn add --dev typescript @types/jest @types/react @types/react-native @types/react-test-renderer`

或
`npm install --save-dev typescript @types/jest @types/react @types/react-native @types/react-test-renderer`

添加一个 TypeScript 配置文件。在项目的根目录中创建一个tsconfig.json：

```json
{
  "compilerOptions": {
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "isolatedModules": true,
    "jsx": "react-native",
    "lib": ["es2017"],
    "types": ["react-native", "jest"],
    "moduleResolution": "node",
    "noEmit": true,
    "strict": true,
    "target": "esnext"
  },
  "exclude": [
    "node_modules",
    "babel.config.js",
    "metro.config.js",
    "jest.config.js"
  ]
}
```

创建一个jest.config.js文件来配置 Jest 以使用 TypeScript:

```js
module.exports = {
  preset: 'react-native',
  moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node']
};
```

将 JavaScript 文件重命名为`* .tsx`:
请保留`./index.js`入口文件，否则将在打包生产版本时遇到问题。

运行 `yarn tsc` 对 TypeScript 文件进行类型检查。
TypeScript 和 React Native 是如何工作的
无需额外配置，和非 TypeScript 的 React Native 项目一样都是直接通过 Babel 体系 将您的文件转换为 JavaScript。我们建议您只使用 TypeScript 编译器的类型检查功能（而不是编译）。如果您有已经存在的 TypeScript 代码需要迁移到 React Native，这里有一些关于使用 Babel 而不是 TypeScript 编译器的注意事项。

### Icon 图标

#### 使用

安装`npm i react-native-vector-icons -D`
项目中引入 `import FontAwesome from 'react-native-vector-icons/FontAwesome'`
使用 `<FontAwesome name="home" size={26} />`
[图标预览]<https://oblador.github.io/react-native-vector-icons>
<https://github.com/oblador/react-native-vector-icons>[github]

#### Android 无法正常显示图标

在 `android/app/build.gradle` （注意：不是 `android/build.gradle`）  下加入

```gradle
project.ext.vectoricons = [
    iconFontNames: [ 'FontAwesome.ttf'] // Name of the font files you want to copy
]

apply from: file("../../node_modules/react-native-vector-icons/fonts.gradle")
```

或者直接引入全部

```gradle
apply from: file("../../node_modules/react-native-vector-icons/fonts.gradle")
```

#### ios报错unRecognized font family 'FontAwesome'

1. 使用Xcode打开项目下的ios文件夹 或者 XXX.xcodeproj 文件（XXX为项目名）
2. 打开之后目录中会有一个与项目名称同名的文件夹，右键单击这个文件夹，选择 Add files to XXX，加入要使用的.ttf文件或者是 react-native-vector-icons下的整个Fonts文件夹，记得勾选上 Create floders 中的 create group 和 Add to targets 中的 XXX。
3. 编辑 与项目名同名的文件夹 下的 info.plist，并加入行 `Fonts provided by application`，在该行中加入 字体文件名
4. 注意，每个被add的.ttf文件都要在 `Fonts provided by application` 中加入，当add的是整个Fonts文件夹时，文件夹中所有.ttf文件都要在`Fonts provided by application` 中加入，否则会出现 We ran "xcodebuild" command but it exited with error code 65. 这样的错误
5. 注意，ios的font-family要求与字体文件字体名相同（不是文件名）比如从阿里妈妈下载的文件 字体名是 iconfont，那么在XXX.js中就要使用iconfont
`const iconSet = createIconSet(glyphMap, 'iconfont', 'MyIcon.ttf'); //阿里妈妈的图标font-family为iconfont`

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

