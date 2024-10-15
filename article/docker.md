---
title: Docker
date: 2024-10-12  12:34:21
categories:
  - linux
tags:
  - docker
  - linux
---

# Docker

## 安装

在 Debian 11 上安装 Docker，你可以按照以下步骤操作：

更新软件包索引：

```bash
sudo apt update
```

安装必要的软件包以允许 apt 通过 HTTPS 使用仓库：

```bash
sudo apt install ca-certificates curl gnupg lsb-release
```

添加 Docker 的官方 GPG 密钥：

```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

设置稳定版仓库：

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

更新软件包索引：

```bash
sudo apt update
```

安装 Docker Engine（社区版）：

```bash
sudo apt install docker-ce docker-ce-cli containerd.io
```

验证 Docker 是否安装成功并正在运行：

```bash
sudo systemctl status docker
```

如果你想让非 root 用户也能运行 Docker 命令，你可以将该用户添加到 docker 组：
你需要注销并重新登录，或者重新启动系统，以确保用户组改变生效。

```bash
sudo usermod -aG docker $USER
```

查看下载好的镜像

```bash
docker images
```

查看运行的 docker 容器：

```bash
docker ps -a
```

停止容器运行

使用 stop 命令，后面可跟容器名，容器 ID

```bash
docker stop mongo
docker stop 1b5c
```

拷贝文件

```bash
docker cp mongo:/data/db /data/mongo/
```

授权文件

```bash
chmod -R 777 /data/mongo/{db,log}
```

删除容器

```bash
docker rm -f mongo
```

## 安装 mongodb

通过 docker 命令拉取 mongo 的镜像，不指定 tag 的话默认是 latest

```bash
docker pull mongo
```

查看 mongo 镜像的文档可知，启动一个 mongo 容器最简单的方式是：
`$ docker run --name mongo -d -p 27017:27017 mongo`
--name mongo：创建的容器的名字，自定义，一般和镜像名字有对应关系
-d：以守护进程方式启动容器
-p 2701:27017：MongoDB 的默认端口号为 27017，这个参数是将宿主机的端口映射到当前 mongo 容器的端口，这样，就能通过网络访问容器的数据库服务了
mongo：使用哪一个镜像创建容器。完整的写法是 `<image:tag>`。由于下载 mongo 镜像时没有指定 tag，也就是使用的默认的 tag，这里也就不用指定了。

### 副本集创建

#### 创建文件夹

在 /opt/mongodb/replica 目录分别创建 27017、27018、27019 三个文件夹，表示存放三个 mongodb 服务的数据。

```bash
mkdir -p /opt/mongodb/replica/27017/data
mkdir -p /opt/mongodb/replica/27017/log
mkdir -p /opt/mongodb/replica/27017/conf

mkdir -p /opt/mongodb/replica/27018/data
mkdir -p /opt/mongodb/replica/27018/log
mkdir -p /opt/mongodb/replica/27018/conf

mkdir -p /opt/mongodb/replica/27019/data
mkdir -p /opt/mongodb/replica/27019/log
mkdir -p /opt/mongodb/replica/27019/conf

```

#### 创建配置文件

分别在 27017、27018、27019 三个文件夹中的 conf 下创建 mongodb.conf 配置文件

```bash
vim /opt/mongodb/replica/27017/conf/mongodb.conf
vim /opt/mongodb/replica/27018/conf/mongodb.conf
vim /opt/mongodb/replica/27019/conf/mongodb.conf
```

如果是 linux 采用 yml 格式，docker 采用 key=vlaue 格式
三类节点创建方式的与普通的单机环境一致，如果都在一台服务器上则设置不同的端口，并且要求副本集的名称一样

- Docker 部署配置
  配置时注意 logpath、dbpath 填写与服务对应的关系，比如操作的文件夹为 27018，那么 logpath、dbpath 中的参数就要为 27018。

```conf
#端口
port=27017
#数据库文件存放目录
dbpath=/opt/mongodb/replica/27017[8|9]/data
#日志文件存放路径
logpath=/opt/mongodb/replica/27017[8|9]/log/mongodb.log
#使用追加方式写日志
logappend=true
#以守护线程的方式运行，创建服务器进程
fork=true
#最大同时连接数
maxConns=100
#不启用验证
#noauth=true
#每次写入会记录一条操作日志
journal=true
#存储引擎有mmapv1、wiredTiger、mongorocks
storageEngine=wiredTiger
#访问IP
bind_ip=0.0.0.0
# 副本集名称
replSet=test
#用户验证，（副本集初始化完毕后再启用，否则初始话会失败）
#auth=true
```

- Linux 本机部署配置：

```yml
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/opt/mongodb/replica/27017[8|9]/log/mongodb.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storag
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 dbPath: "/opt/mongodb/replica/27017[8|9]/data"
 journal:
    #启用或禁用持久性日志以确保数据文件保持有效和可恢复。
    enabled: true
processManagement:
 #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
net:
 #服务实例绑定的IP，默认是localhost，多个ip逗号分割
 bindIp: 0.0.0.0
 #绑定的端口，默认是27017
 port: 27017[8|9]
security:
#用户验证
# authorization: enabled
# 副本集名称
replication:
  replSetName: test

```

#### 启动

- docker 启动
  其中`--replSet` 表示副本的名称，相同的名称组副本集

```bash
docker run -d -p 27017:27017 \
-v /opt/mongodb/replica/27017/data:/data/db \
-v /opt/mongodb/replica/27017/conf:/data/conf \
-v /opt/mongodb/replica/27017/datalog:/data/log \
--name mongodb-27017  docker.zhai.cm/library/mongo \
--replSet "test"

docker run -d -p 27018:27017 \
-v /opt/mongodb/replica/27018/data:/data/db \
-v /opt/mongodb/replica/27018/conf:/data/conf \
-v /opt/mongodb/replica/27018/datalog:/data/log \
--name mongodb-27018  docker.zhai.cm/library/mongo \
--replSet "test"

docker run -d -p 27019:27017 \
-v /opt/mongodb/replica/27019/data:/data/db \
-v /opt/mongodb/replica/27019/conf:/data/conf \
-v /opt/mongodb/replica/27019/datalog:/data/log \
--name mongodb-27019  docker.zhai.cm/library/mongo \
--replSet "test"
```

- 本机启动

```bash
mongod --config ../replica/27017[8|9]/conf/mongodb.conf
```

#### 初始化

进入容器

```bash
#注意mongoDB的版本，6.0之前用mongo客户端命令
docker exec -it mongodb-27017 bash

# 本地启动
#mongo --host=127.0.0.1 --port=27017


#6.0之后使用mongosh客户端命令
mongosh --host 127.0.0.1 --port 27017
```

```bash
# 初始化
#_id为副本集名称，对应--replSet后的名称
#27017为主节点，27018为从节点，27019为仲裁节点，可以配置多个从节点和仲裁节点
rs.initiate({_id:"test",members:[{_id:0,host:"139.9.66.30:27017",priority:3},{_id:1,host:"139.9.66.30:27018",priority:2},{_id:2,host:"139.9.66.30:27019",arbiterOnly:true}]})
```

通过 `rs.status()`可以看到每个节点的角色情况

## 问题

```bash
Error response from daemon: Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
```

网络问题，指定源下载
`docker pull docker.zhai.cm/library/mysql:latest`
