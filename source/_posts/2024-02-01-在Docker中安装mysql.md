---
title: 在Docker中安装mysql
tags:
  - Docker
  - mysql
  - 服务器运维
categories:
  - 服务器运维
date: 2024-02-01 06:46:48
description:
---

### 1 mysql镜像拉取

docker pull mysql:5.7

### 2 容器启动

```
docker run -p 10241:3306 --name mysql \

-v /usr/local/SDK_YPT/mysql/log:/var/log/mysql \

-v /usr/local/SDK_YPT/mysql/data:/var/lib/mysql \

-v /usr/local/SDK_YPT/mysql/conf:/etc/mysql/conf.d \

-e MYSQL_ROOT_PASSWORD=Ypt1024mysql \

-d mysql:5.7
```

### 3 添加mysql配置文件

进入Linux 中，在自己指定的mysql配置文件路径下，新建配置文件 my.cnf， 拷贝以下内容

```
[client]
default-character-set=utf8mb4

[mysql]
default-character-set=utf8mb4

[mysqld]
init_connect='SET NAMES utf8mb4'
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
```

### 4 设置mysql自启

```
docker update --restart=always mysql
```

### 5 安装成功测试

#### 1 连接mysql测试

![image-20240201070708595](http://cdn.this0.com/blog/img/image-20240201070708595.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000000&Signature=N84XJcuxwGvnERTbXnSwXtSZfr0=&x-oss-process=style/cdn.this0)

#### 2 查看字符集

```
SHOW VARIABLES LIKE 'character%'
```

![image-20240201072817525](http://cdn.this0.com/blog/img/image-20240201072817525.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000001&Signature=CVgKJOd/rtq1RSvuojBUE7uPAjs=&x-oss-process=style/cdn.this0)
