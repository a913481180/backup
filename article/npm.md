---
title:  npm仓库
date: 2022-06-11 20:22:22
categories:
- linux
---

###
一，包的安装/更新/卸载
npm i 或者 npm install
装包,默认安装最新版本，直接npm i 会安装package.json中dependencies的所有包

npm i xx@1.0
安装指定版本

npm i -S 或者 npm i --save
装包到生产环境（也就是上线后需要的依赖），在package.json的dependencies中生成版本信息

npm i -D 或者 npm i --save-dev
装包到开发环境（也就是开发时需要的依赖），在package.json的devDependencies中生成版本信息

npm i xx@1.0 --save-exact
精确安装指定版本的包到生产环境。精确的意思就是，什么版本就是什么版本，版本号前面的^ 会消失掉。有^ 代表是补丁版本

npm uninstall xx
卸载包

npm update xx@1.0
更新包，默认更新到最新

npm outdated
检查包是否过时，默认列出所有过时的包

npm i xx1@npm:xx@1
npm i xx2@npm:xx@2
安装同一个包的不同版本，如要引入1版本，则import ‘xx1’。npm 6.9及以后的版本可用

二，信息查看
npm ls
查看安装过的包，加-g就是查看全局安装过的包， --depth 0就是查看第一层，不再深入递归查询出子文件夹

npm ls 包名 -g
查看某个包，有则显示版本，没有则是empty

npm root xx
查看包的安装路径

npm view xx versions
查看某个包在线上的所有版本

三，配置向
npm config set proxy="http://xx.com:8080"
因为公司的防火墙原因，无法完成任何模块的安装，这个时候设置代理可以解决

npm config set registry="https://registry.npm.taobao.org"
设置安装源，如上就是设置源为国内的淘宝镜像

npm config list -l
查看所有配置

### 发布自己的包
第一步：进入要发布的项目根目录，初始化为npm包：

npm init

依次按提示填入包名、版本、描述、github地址、关键字、license等
这步完成之后会生成一个package.json文件，上面输入的这些信息可以在该文件中修改
name是名字，不能和别人的一样，否则发布失败
version是版本号，每次发布自己手动修改，版本号需递增，否则发布失败
main设置入口文件，入口文件需和package同级，如不同级，则应该写/xx/yy这样的相对路径，

注意：如果你的包引用了第三方包，则需要在package.json文件种增加dependencies节点，写入依赖的包及版本
"dependencies": {
    "colors": "^1.3.2",
    "on-finished": "^2.3.0"
  }

第二步、注册npm用户，有两种方法  注册npm用户，有两种方法

方法一、npm官网注册：npm

方法二、使用npm 命令注册：npm adduser

第三步、账号登录

npm login

依次输入第二步中第一种方法注册的用户名、密码和邮箱
第四步、发布包，上传到npm包服务器

npm publish

注意：如果报错：'You do not have permission to publish "mypackage1". Are you logged in as the correct user?'

表示包’mypackage1‘已经在包管理器已经存在被别人用了，需要更该包名称
 更新一个已经发布的包
第一步、修改包的版本

：这次我在包根目录下新加了一个index.js文件

npm version patch  该命令在原来的版本上自动加1,实际上是将package.json文件中的version值修改了。
第二步、重新发布包

npm publish

三、删除包
1、删除指定的版本

npm unpublish 包名@版本号

2、删除整个包

npm unpublish 包名 --force

### 五，其它
npm cache clean
清除npm本地缓存
npm init
初始化package.json
npm start
"scripts": {
    "start": "xxx"
}
启动指令,npm start指令会匹配到package的scripts里的配置，然后启动对应脚本。start此类的支持自定义，常见的自定义为dev

### yarn：
>Yarn 是由 Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具 。
>《淘宝 NPM 镜像站喊你切换新域名啦》 https://npm.taobao.org 和 https://registry.npm.taobao.org 将在 2022.06.30 号正式下线和停止 DNS 解析。
>域名切换规则：
>https://npm.taobao.org => https://npmmirror.com
>https://registry.npm.taobao.org => https://registry.npmmirror.com

安装：
npm install -g yarn

执行 yarn xxx 命令报以下错误时：

操作以下步骤进行策略更改：

使用管理员身份运行 cmd 或 Power shell
输入：set-executionpolicy remotesigned
输入”Y“，回车保存
yarn 配置源：
// 查询源
yarn config get registry

// 更换国内源
yarn config set registry https://registry.npmmirror.com

// 恢复官方源
yarn config set registry https://registry.yarnpkg.com

// 删除注册表
yarn config delete registry

npm 配置源：
注意 npm 更换国内镜像源之后，将无法再使用 npm search 命令，需要恢复为官方源才可以使用，如果恢复官方源后还不可使用，运行删除注册表命令后重试即可。

// 查询源
npm config get registry

// 更换国内源
npm config set registry https://registry.npmmirror.com

// 恢复官方源
npm config set registry https://registry.npmjs.org

// 删除注册表
npm config delete registry