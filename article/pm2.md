---
title: PM2
date: 2023-01-11 20:22:22
categories:
  - linux
---

# pm2

PM2 是常用的 node 进程管理工具，它可以提供 node.js 应用管理，如自动重载、性能监控、负载均衡等。同类工具有 Supervisor、Forever 等。

pm2 是一个进程管理工具,可以用它来管理你的 node 进程，并查看 node 进程的状态，当然也支持性能监控，进程守护，负载均衡等功能。pm2 基本是 Nodejs 应用程序不二的守护进程选择，事实上它并不仅仅可以启动 Nodejs 的程序，只要是一般的脚本的程序它同样可以胜任。

## 特性

- 内建负载均衡（使用 node cluster 集群模块，可以使用服务器上的所有 cpu）

- 后台运行（node app.js 这种命令是直接在前台运行的，不稳定，很容易断）

- 0 秒停机重载（应该是上线升级的时候 不需要停机）

- 停止不稳定的进程（避免无限循环）

- 控制台检测

## 安装

`npm install -g pm2`

## 启动 PM2 项目

pm2 支持直接运行 server.js 启动项目，如下：`pm2 start server.js`

- `--watch`：监听应用目录的变化，一旦发生变化，自动重启。如果要精确监听、不见听的目录，最好通过配置文件。
- `-i --instances`：启用多少个实例，可用于负载均衡。如果-i 0 或者-i max，则根据当前机器核数确定实例数目。
- `--ignore-watch`：排除监听的目录/文件，可以是特定的文件名，也可以是正则。比如`--ignore-watch="test node_modules "some scripts""`
- `-n --name`：应用的名称。查看应用信息的时候可以用到。
- `-o --output <path>`：标准输出日志文件的路径。
- `-e --error <path>`：错误输出日志文件的路径。
- `--interpreter <interpreter>`：the interpreter pm2 should use for executing app (bash, python...)。比如你用的 coffee script 来编写应用。

## 文件夹结构

PM2 启动后，它将自动创建这些文件夹：

```
$HOME/.pm2：将包含所有PM2相关文件

$HOME/.pm2/logs：将包含所有应用程序日志

$HOME/.pm2/pids：将包含所有应用程序pids

$HOME/.pm2/pm2.log：PM2 日志

$HOME/.pm2/pm2.pid：PM2 pid

$HOME/.pm2/rpc.sock：远程命令的套接字文件

$HOME/.pm2/pub.sock：可发布事件的套接字文件

$HOME/.pm2/conf.js：PM2配置
```

在 Windows 中，$ HOME 环境变量可以是$ HOMEDRIVE + $ HOMEPATH

## 常用命令

- 查看应用列表（查看当前机器执行的所有进程）
  `pm2 list/ls/l`
- 查看某个应用详情（查看某个进程信息）
  `pm2 show app_name|app_id`或者`pm2 describe app_name|app_id`
- 重启
  `pm2 restart app.js`或`pm2 restart app_name|app_name`
- 停止
  `pm2 stop app_name|app_id`或停止全部`pm2 stop all`
- 删除
  类似 pm2 stop，如下
  `pm2 delete app_name|app_id`
  `pm2 delete all`
- 日志查看
  除了可以打开日志文件查看日志外，还可以通过`pm2 logs`来查看实时日志。这点对于线上问题排查非常重要。比如某个 node 服务突然异常重启了，那么可以通过 pm2 提供的日志工具来查看实时日志，看是不是脚本出错之类导致的异常重启。查看指定应用程序日志：`pm2 logs app_name|app_id`显示所有应用程序的日志：`pm2 logs`

## 负载均衡

命令如下，表示开启三个进程。如果`-i 0`，则会根据机器当前核数自动开启尽可能多的进程。

`pm2 start app.js -i 3` # 开启三个进程
`pm2 start app.js -i max `# 根据机器 CPU 核数，开启对应数目的进程

## 开机自动启动

可以通过`pm2 startup`来实现开机自启动。细节可参考。大致流程如下

通过`pm2 save`保存当前进程状态。

通过`pm2 startup [platform]`生成开机自启动的命令。（记得查看控制台输出）

将步骤 2 生成的命令，粘贴到控制台进行，搞定。

## 监控 CPU/内存

运行如下命令，查看当前通过 pm2 运行的进程的状态。即可监控 CPU 和内存的使用情况，同时应用的报错信息也会打印在 Global Logs 窗口中，如下：

`pm2 monit`

## 内存使用超过上限自动重启

如果想要你的应用，在超过使用内存上限后自动重启，那么可以加上--max-memory-restart 参数。（有对应的配置项）

`pm2 start big-array.js --max-memory-restart 20M`

## pm2 编程接口

如果想把 pm2 的进程监控，跟其他自动化流程整合起来，pm2 的编程接口就很有用了。细节可参考官方文档：
`http://pm2.keymetrics.io/docs/usage/pm2-api/`
