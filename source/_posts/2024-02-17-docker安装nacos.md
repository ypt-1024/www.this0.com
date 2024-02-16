---
title: docker安装nacos
tags:
  - null
categories:
  - null
date: 2024-02-17 02:53:13
description:
---

### 1 新建nacos文件目录

```
mkdir -p /usr/local/SDK_YPT/nacos/logs/                      #新建logs目录

mkdir -p /usr/local/SDK_YPT/nacos/init.d/         
```

### 2 修改配置文件

vim /usr/local/SDK_YPT/nacos/init.d/custom.properties       

拷贝以下内容：

```properties
docker run -d  \
-e MODE=standalone \
-e PREFER_HOST_MODE=hostname \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-e MYSQL_SERVICE_HOST=你的数据库ip地址 \
-e MYSQL_SERVICE_PORT=数据库端口 \
-e MYSQL_SERVICE_USER=数据库用户名 \
-e MYSQL_SERVICE_PASSWORD=数据库密码 \
-e MYSQL_SERVICE_DB_NAME=数据库名 \
-p 服务器nacos端口:8848 --name nacos \
--name nacos \
--restart=always \
nacos/nacos-server:1.4.1
```

