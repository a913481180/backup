---
title: 前端包管理工具
date: 2022-06-11 20:22:22
categories:
  - linux
tags:
  - 前端
---

## NPM

### 一，包的安装/更新/卸载

`npm i` 或者`npm install`装包,默认安装最新版本，直接`npm i`会安装`package.json`中 dependencies 的所有包

`npm i xx@1.0`安装指定版本

`npm i -S` 或者 `npm i --save`装包到生产环境（也就是上线后需要的依赖），在`package.json`的 dependencies 中生成版本信息

`npm i -D` 或者 `npm i --save-dev`装包到开发环境（也就是开发时需要的依赖），在`package.json`的 devDependencies 中生成版本信息

`npm i xx@1.0 --save-exact`精确安装指定版本的包到生产环境。精确的意思就是，什么版本就是什么版本，版本号前面的`^`会消失掉。有`^` 代表是补丁版本

`npm uninstall xx`卸载包

`npm update xx@1.0`更新包，默认更新到最新

`npm outdated`检查包是否过时，默认列出所有过时的包

`npm i xx1@npm:xx@1`
`npm i xx2@npm:xx@2`
安装同一个包的不同版本，如要引入 1 版本，则`import ‘xx1’`。`npm 6.9`及以后的版本可用

### 二，信息查看

`npm ls`查看安装过的包，加`-g`就是查看全局安装过的包， `--depth 0`就是查看第一层，不再深入递归查询出子文件夹

`npm ls 包名 -g`查看某个包，有则显示版本，没有则是 empty

`npm root xx`查看包的安装路径

`npm view xx versions`查看某个包在线上的所有版本

三，配置向
`npm config set proxy="http://xx.com:8080"`因为公司的防火墙原因，无法完成任何模块的安装，这个时候设置代理可以解决

`npm config set registry="https://registry.npm.taobao.org"`设置安装源，如上就是设置源为国内的淘宝镜像

> <https://npm.taobao.org> 和 <https://registry.npm.taobao.org> 将在 2022.06.30 号正式下线和停止 DNS 解析。
> 新域名：<https://registry.npmmirror.com>

注意 npm 更换国内镜像源之后，将无法再使用 npm search 命令，需要恢复为官方源才可以使用，如果恢复官方源后还不可使用，运行删除注册表命令后重试即可。

`npm config delete registry`// 删除注册表

`npm config list -l`查看所有配置

### 三, 其它

`npm cache clean`清除 npm 本地缓存

`npm init`初始化 package.json

`npm start`启动指令,npm start 指令会匹配到 package 的 scripts 里的配置，然后启动对应脚本。start 此类的支持自定义，常见的自定义为 dev

```json
"scripts": {
    "start": "xxx"
}
```

### 四，发布自己的包

#### 第一步：进入要发布的项目根目录，初始化为 npm 包

`npm init`

依次按提示填入包名、版本、描述、github 地址、关键字、license 等
这步完成之后会生成一个 package.json 文件，上面输入的这些信息可以在该文件中修改
name 是名字，不能和别人的一样，否则发布失败
version 是版本号，每次发布自己手动修改，版本号需递增，否则发布失败
main 设置入口文件，入口文件需和 package 同级，如不同级，则应该写/xx/yy 这样的相对路径，

注意：如果你的包引用了第三方包，则需要在 package.json 文件种增加 dependencies 节点，写入依赖的包及版本

```json
"dependencies": {
    "colors": "^1.3.2",
    "on-finished": "^2.3.0"
  }
```

#### 第二步、注册 npm 用户，有两种方法 注册 npm 用户，有两种方法

方法一、npm 官网注册：npm

方法二、使用 npm 命令注册：npm adduser

#### 第三步、账号登录

`npm login`

依次输入第二步中第一种方法注册的用户名、密码和邮箱

#### 第四步、发布包，上传到 npm 包服务器

`npm publish`

注意：如果报错：`'You do not have permission to publish "mypackage1". Are you logged in as the correct user?'`

表示包`mypackage1`已经在包管理器已经存在被别人用了，需要更该包名称

### 五，更新一个已经发布的包

#### 第一步、修改包的版本

这次我在包根目录下新加了一个 index.js 文件

`npm version patch` 该命令在原来的版本上自动加 1,实际上是将 package.json 文件中的 version 值修改了。

#### 第二步、重新发布包

`npm publish`

### 六、删除包

1、删除指定的版本

`npm unpublish 包名@版本号`

2、删除整个包

`npm unpublish 包名 --force`

## YARN

> Yarn 是由 Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具 。

安装：`npm install -g yarn`

执行 `yarn xxx` 命令报以下错误时：

操作以下步骤进行策略更改：

> 1.使用管理员身份运行 `cmd` 或 `Power shell` 2.输入：`set-executionpolicy remotesigned` 3.输入`Y`，回车保存

yarn 配置源：

```bash
// 查询源
yarn config get registry

// 更换国内源
yarn config set registry https://registry.npmmirror.com

// 恢复官方源
yarn config set registry https://registry.yarnpkg.com

// 删除注册表
yarn config delete registry
```

npm 与 yarn 命令对比：

|npm 命令|yarn 命令|说明|
|-|-|-|
|npm install | yarn install|安装依赖,安装时，如果 node_modules 中有相应的包则不会重新下载 --force 可以强制|
|npm install [package@版本号] | yarn add [package@版本号]|指定版本安装一个包|
|npm install [package] --save | yarn add [package]|安装一个包,--save 是 yarn 默认的|
|npm install -g [package] | yarn global add [package@版本号]|指定版本安装一个包|
|npm install --save-dev [package] | yarn add --dev [package]|安装包作为开发依赖项，简写`-D`|
|npm uninstall [package] | yarn remove [package]|卸载一个包|
|npm uninstall --save-dev [package] | yarn remove [package]|卸载开发依赖包|
|npm update | yarn upgrade|更新的依赖关系|
|npm update [package] | yarn upgrade [package]|更新包，npm 可以通过 ‘--save|--save-dev’ 指定升级哪类依赖|

## PNPM

> pnpm(Performance npm，简称高性能 npm)的作者 Zoltan Kochan 发现 yarn 并没有打算去解决上述的这些问题，于是另起炉灶，写了全新的包管理器。

pnpm 复刻了 npm 所有的命令，所以使用方法和 npm 一样，并且在安装目录结构上做了优化，特点是善用链接，且由于链接的优势，大多数情况下 pnpm 的安装速度比 yarn 和 npm 更快。

安装：`npm i pnpm -g`

升级版本`pnpm add -g pnpm to update`

设置源：

```bash
pnpm config get registry //查看源
pnpm config set registry https://registry.npmmirror.com //切换淘宝源
```

### 原理

在项目根目录安装依赖包 bar，而 bar 依赖了 foo。点开项目根目录的 node_modules，可以看到我们在项目中安装的 bar，以及.pnpm 文件夹。
那么 pnpm 是如何维护依赖的嵌套关系的呢？

首先在`.pnpm`文件夹以平铺的方式存储了项目中所有的依赖包，建立硬链接。命名的方式如下：

`.pnpm/<organization-name>+<package-name>@<version>/node_modules/<name>// 组织名(若无会省略)+包名@版本号/node_modules/名称(项目名称)`
其中.pnpm 中的包中的 node_modules 下面的包中的每个文件都是内容可寻址存储的硬链接。将 foo 和 bar 都放到 node_modules 文件夹，可以保证包能自行导入自己。

```bash
node_modules
└── .pnpm
 ├── foo@1.0.0
 │   └── node_modules
 │       └── foo -> <store>/bar 硬链接
 │           ├── index.js
 │           └── package.json
 └── bar@1.0.0
     └── node_modules
         └── bar -> <store>/bar 硬链接
             ├── index.js
             └── package.json
```

建立符号链接，由于 bar 依赖了 foo，因此 foo 被符号链接到bar@1.0.0/node_modules 文件夹下，

```bash
node_modules
└── .pnpm
 ├── foo@1.0.0
 │   └── node_modules
 │       └── foo -> <store>/bar 硬链接
 │           ├── index.js
 │           └── package.json
 └── bar@1.0.0
     └── node_modules
         └── bar -> <store>/bar 硬链接
         │    ├── index.js
         │    └── package.json
         └── foo -> ../../bar@1.0.0/node_modules/bar 符号链接
```

处理直接依赖，bar 被符号链接到根目录的 node_modules 文件夹

```bash
node_modules
├── bar -> ./.pnpm/bar@1.0.0/node_modules/bar 符号链接
└── .pnpm
 ├── foo@1.0.0
 │   └── node_modules
 │       └── foo -> <store>/bar 硬链接
 │           ├── index.js
 │           └── package.json
 └── bar@1.0.0
     └── node_modules
         └── bar -> <store>/bar 硬链接
         │    ├── index.js
         │    └── package.json
         └── foo -> ../../bar@1.0.0/node_modules/bar 符号链接
```

在项目中我们引入 bar 的时候，会在当前项目根目录的 node_modules 找 bar，从 projectRoot/node_modules/bar 这个符号链接找到 bar 的真实文件 projectRoot/node_modules/.pnpm/bar@1.0.0/node_modules/bar（硬链接即可看做真实存在的文件）。
接着 bar 依赖了 foo，根据 node 的寻址机制，会从里到外找 foo，最后在这个路径 projectRoot/node_modules/.pnpm/bar@1.0.0/node_modules/foo 找到了，这个是一个符号链接，会定位到 projectRoot/node_modules/.pnpm/foo@1.0.0

### 优势

- 解决了幽灵依赖的问题
  yarn 和 npm 中的幽灵依赖：由于在 yarn 或 npm 中采用扁平化的安装包的方式，即所有的包（包ffff依赖）都被提升到了项目根目录中的 node_modules 中，那么项目中可以直接引用到不在 package.json 中声明的包，那么在包更新的时候，可能会导致问题。
  而 pnmp 中的项目根目录的 node_modules 中只有项目直接依赖的包，所以不会有这个问题。

- 路径过长的问题
  在 npm@3 之前，npm 采用的嵌套结构管理包，node_modules 结构是干净、可预测的，因为 node_modules 中的每个依赖项都有自己的 node_modules 文件夹。但是这样的管理方式会导致路径过程超出 window 的限制。
  在 npm@3 和 yarn 中，采用了扁平化结构管理包（解决了路径过长的问题，导致了幽灵依赖的问题）。
  pnpm 利用符号链接解决了这个问题。
  继续上面的
  添加qar@2.0.0作为 foo 和 bar 的依赖项,可以看到我们目录深度没有改变，但却表达了嵌套的包依赖关系。

```bash
node_modules
├── bar -> ./.pnpm/bar@1.0.0/node_modules/bar 符号链接
└── .pnpm
    ├── foo@1.0.0
    │   └── node_modules
    │       └── foo -> <store>/bar 硬链接
    │           ├── index.js
    │           └── package.json
    │       └── qar -> ../../qar@2.0.0/node_modules/qar 符号链接
    └── bar@1.0.0
    │   └── node_modules
    │       └── bar -> <store>/bar 硬链接
    │       │    ├── index.js
    │       │    └── package.json
    │        └── foo -> ../../bar@1.0.0/node_modules/bar 符号链接
    │       └── qar -> ../../qar@2.0.0/node_modules/qar 符号链接
    └── qar@2.0.0
            └── node_modules
                └── qar -> <store>/qar
```

- 占用更小的内存，下载速度更快
  使用 npm 时，依赖每次被不同的项目使用，都会重复安装一次。 而在使用 pnpm 时，依赖会被存储在内容可寻址的存储中，所以：

如果你用到了某依赖项的不同版本，只会将不同版本间有差异的文件添加到仓库。 例如，如果某个包有 100 个文件，而它的新版本只改变了其中 1 个文件。那么 pnpm update 时只会向存储中心额外添加 1 个新文件，而不会因为仅仅一个文件的改变复制整新版本包的内容。
所有文件都会存储在硬盘上的某一位置。 当软件包被被安装时，包里的文件会硬链接到这一位置，而不会占用额外的磁盘空间。 这允许你跨项目地共享同一版本的依赖。
因此，您在磁盘上节省了大量空间，这与项目和依赖项的数量成正比，并且安装速度要快得多！

## CNPM

cnpm 是个中国版的 npm，是淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm

## npm vs yarn vs pnpm

### npm v1-v2

初代 npm(Node.js Package Manager)随着 Node.js 的发布出现了。

它的文件结构是嵌套的,这会导致 3 个问题：

1、node_modules 体积过大(大量重复的包被安装)

2、node_modules 嵌套层级过深(会导致文件路径过长的问题)

3、模块实例不能共享

### yarn & npm v3

这个版本 yarn 和 npm v3 带来了扁平化依赖管理：
扁平化处理时，比如安装 A，A 依赖 B 和 C，C 依赖 D 和 E，就把 A~E 全部放到 node_modules 目录下，从而解决上个版本中 node_modules 嵌套层级过深的问题。

在 install 安装时，会不停的往上级 node_modules 中寻找，如果找到同样的包，就不再重复安装，从而解决了大量包被重复安装的问题。但是扁平化带来了新的问题：

1. 依赖结构的不确定性
2. 扁平化算法本身复杂性很高，耗时较长
3. 项目中仍然可以非法访问没有声明过依赖的包

 对于问题 1，比如 B 和 C 都依赖了 F，但是依赖的 F 版本不一样,依赖结构的不确定性表现是扁平化的结果不确定，以下 2 种情况都有可能，取决于 package.json 中 B 和 C 的位置。于是出现 yarn.lock(npm5 才有 package-lock.json)，来保证 install 后产生确定的依赖结构。但这并不能完全解决问题，node_modules 中依然存在各种不同版本的 F，而这可能导致各种情况的编译报错，以及安装满，占磁盘空间。

对于问题 3，package.json 中我们只声明了 A，B~F 都是因为扁平化处理才放到和 A 同级的 node_modules 下，理论上在项目中写代码时只可以使用 A，但实际上 B~F 也可以使用，由于扁平化将没有直接依赖的包提升到 node_modules 一级目录，Node.js 没有校验是否有直接依赖，所以项目中可以非法访问没有声明过依赖的包。这会产生两个问题：B~F 中的包升级后，项目可能出问题和额外的管理成本(比如协作时别人运行一次 npm install 后项目依旧跑不起来)

### pnpm

pnpm 复刻了 npm 所有的命令，所以使用方法和 npm 一样，并且在安装目录结构上做了优化，特点是善用链接，且由于链接的优势，大多数情况下 pnpm 的安装速度比 yarn 和 npm 更快。

比如安装 A，A 依赖了 B：

1. 安装依赖
   A 和 B 一起放到.pnpm 中(和上面相比，这里没有耗时的扁平化算法)。
   另外A@1.0.0下面是 node_modules，然后才是 A，这样做有两点好处：

   - 允许包引用自身
   - 把包和它的依赖摊平，避免循环结构

2. 处理间接依赖
   A 平级目录创建 B，B 指向B@1.0.0下面的 B。

3. 处理直接依赖
   顶层 node_modules 目录下创建 A，指向A@1.0.0下的 A。

### 总结

- 如果你想更快的速度，更小的空间，你应该选择 pnpm;

- 如果你要用 Monorepo(monolithic repository)是一种项目架构,简单的来说:一个仓库内包含多个开发项目(模块,包)，你可以用 yarn 或 pnpm;

- 如果是 node 项目，你应该用 npm，因为这是 node 官方推荐的，而且 yarn 不支持 node5+;

- 对于 npm 项目，如果你担心项目的安全性，你可以考虑用 yarn 替换 npm。
