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

```properties
server.contextPath=/nacos
server.servlet.contextPath=/nacos
server.port=10242  #nacos端口

spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://数据库ip:端口/数据库名?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true #这里需要修改端口
db.user=root #数据库用户名
db.password=密码 #数据库密码

nacos.cmdb.dumpTaskInterval=3600
nacos.cmdb.eventTaskInterval=10
nacos.cmdb.labelTaskInterval=300
nacos.cmdb.loadDataAtStart=false
management.metrics.export.elastic.enabled=false
management.metrics.export.influx.enabled=false
server.tomcat.accesslog.enabled=true
server.tomcat.accesslog.pattern=%h %l %u %t "%r" %s %b %D %{User-Agent}i
nacos.security.ignore.urls=/,/**/*.css,/**/*.js,/**/*.html,/**/*.map,/**/*.svg,/**/*.png,/**/*.ico,/console-fe/public/**,/v1/auth/login,/v1/console/health/**,/v1/cs/**,/v1/ns/**,/v1/cmdb/**,/actuator/**,/v1/console/server/**
nacos.naming.distro.taskDispatchThreadCount=1
nacos.naming.distro.taskDispatchPeriod=200
nacos.naming.distro.batchSyncKeyCount=1000
nacos.naming.distro.initDataRatio=0.9
nacos.naming.distro.syncRetryDelay=5000
nacos.naming.data.warmup=true
nacos.naming.expireInstance=true
```

### 3 启动nacos容器

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

