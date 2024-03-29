---
title: 用docker搭建开发环境
date: 2022-08-16 21:38:23
updated: 2022-10-04 23:58:05
tags:
- docker
---

> 说明：系统Ubuntu20.04或WSL2 Ubuntu20.04，docker版本20+

## docker-compose安装

对于简单开发环境来说使用docker-compose比直接使用docker命令略微方便一点，当然集群部署还是得k8s。

下载地址：https://github.com/docker/compose/releases

下载后重命名为docker-compose，然后修改权限为755并移动到`/usr/local/bin`目录下

```shell
chmod 755 docker-compose
mv docker-compose /usr/local/bin
```

## 1、mysql

pull需要的版本的mysql的docker镜像
```shell
docker pull mysql:5.7
```

先run创建并启动容器
```shell
docker run --name mysql mysql:5.7
```

拷贝容器中的my.cnf到外部
```shell
docker cp [容器ID]:/etc/mysql/my.cnf ./
```

如果拷贝失败可能是容器内没有`/etc/mysql/my.cnf`，配置可能在`/etc/my.cnf`或`/usr/local/etc/mysql/my.cnf`

具体需要到容器内部查看
```shell
docker exec -it [容器ID] /bin/bash
```

停止并删除容器
```shell
docker stop [容器ID]
docker rm [容器ID]
```

准备好my.cnf后创建一个目录用于存放配置文件和数据文件
```shell
mkdir -p ./mysql/data
mv my.cnf ./mysql
```

正式创建并启动我们需要的容器
```shell
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -e "TZ=Asia/Shanghai" -v /home/XXXX/mysql/data:/var/lib/mysql -v /home/XXXX/mysql/my.cnf:/etc/mysql/my.cnf -d mysql:5.7
```

还可以使用docker-compose来启动
docker-compose.yml内容如下：
```yaml
version: '3'
services:
  mysql:
    image: mysql:5.7
    volumes:
      - ./mysql/my.cnf:/etc/mysql/my.cnf
      - ./mysql/data:/var/lib/mysql
    environment:
      - "MYSQL_ROOT_PASSWORD=123456"
      - "TZ=Asia/Shanghai"
    ports:
      - 3306:3306
    restart: always
```

在yml文件同目录下创建并启动容器
```shell
docker-compose up -d
```


## 2、redis

pull最新版本的redis的镜像
```shell
docker pull redis:latest
```

下载最新版本的redis：https://redis.io/download/

解压取出redis.conf文件

创建目录用来存储redis配置文件和持久化的数据，并将准备好的redis.conf放到目录下
```shell
mkdir -p ./redis/data
mv ./redis.conf ./redis
```

创建并启动容器
```shell
docker run --name redis -p 6379:6379 -v /home/XXXX/redis/data:/data -v /home/XXXX/redis/redis.conf:/etc/redis/redis.conf -d redis:latest redis-server /etc/redis/redis.conf
```

可以选择docker-compose启动

docker-compose.yml如下
```yaml
version: '3'
services:
  redis:
    image: redis:latest
    ports:
      - 6379:6379
    volumes:
      - ./redis/data:/data
      - ./redis/redis.conf:/etc/redis/redis.conf
    command: redis-server /etc/redis/redis.conf
    restart: always
```

在yml文件同目录下创建并启动容器
```shell
docker-compose up -d
```

## 3、FTP

拉取镜像

```shell
docker pull fauria/vsftpd
```

创建并启动容器

```shell
docker run -p 2121:21 -p 2020:20 -p 21100-21110:21100-21110 \
-e FTP_USER=test -e FTP_PASS=123456 -e PASV_ADDRESS=<X.X.X.X> -e PASV_MIN_PORT=21100 -e PASV_MAX_PORT=21110 \
-v /home/XXXX/vsftpd:/home/vsftpd \
--name vsftpd \
--restart=always \
-d fauria/vsftpd
```

`<X.X.X.X>`要更换成主机ip，使用`ifconfig`查看，例如PASV_ADDRESS=10.10.10.10

`/home/XXXX/vsftpd`中的XXXX更换为当前用户名，或者自行制定目录。

这里使用主机的2121是为了防止与主机的21端口冲突，如果已经明确21端口和20端口没有被占用，那么这里可以使用21和20端口。

使用docker-compose方式：

`<X.X.X.X>`替换成主机ip

```yaml
version: '3'
services:
  vsftpd:
    image: fauria/vsftpd
    ports:
      - 2121:21
      - 2020:20
      - 21100-21110:21100-21110
    volumes:
      - ./vsftpd:/home/vsftpd
    environment:
      - "FTP_USER=test"
      - "FTP_PASS=123456"
      - "PASV_ADDRESS=<X.X.X.X>"
      - "PASV_MIN_PORT=21100"
      - "PASV_MAX_PORT=21110"
    restart: always
```