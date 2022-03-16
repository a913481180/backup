---
title:  npm仓库
date: 2022-06-11 20:22:22
categories:
- linux
---
第一步：进入要发布的项目根目录，初始化为npm包：

npm init

依次按提示填入包名、版本、描述、github地址、关键字、license等
这步完成之后会生成一个package.json文件，上面输入的这些信息可以在该文件中修改

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