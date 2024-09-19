---
title: 在Docker中安装mysql
tags:
  - Docker
  - mysql
  - 服务器运维
categories:
  - 服务器运维
abbrlink: ee432f4
date: 2024-02-01 06:46:48
description:
---

### 1 mysql镜像拉取

docker pull mysql:5.7

### 2 添加mysql配置文件

#### 1 创建数据存放目录

```
mkdir -p /usr/local/SDK_YPT/mysql/conf
```

#### 2 创建配置文件

在刚创建的目录下新建配置文件 my.cnf

```
vim /usr/local/SDK_YPT/mysql/conf/my.cnf
```

拷贝如下内容：

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

### 3 容器启动

*`映射文件目录里不要有以前的数据库残留文件`

```
docker run -p 映射端口:3306 --name mysql -v /usr/local/SDK_YPT/mysql/log:/var/log/mysql -v /usr/local/SDK_YPT/mysql/data:/var/lib/mysql -v /usr/local/SDK_YPT/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=设置密码 -d mysql:5.7
```

### 4 设置mysql自启

```
docker update --restart=always mysql
```

### 5 安装成功测试

#### 1 连接mysql测试

![image-20240201070708595](https://blog-resources.this0.com/image/202405082036678.png?x-oss-process=style/this0-blog)

#### 2 查看字符集

```
SHOW VARIABLES LIKE 'character%'
```

![image-20240201072817525](https://blog-resources.this0.com/image/202405082037348.png?x-oss-process=style/this0-blog)

### 6 授予远程访问权限

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION;
```

远程登陆测试

```
mysql -h  你的主机ip -P mysql端口号 -u root -p
```

![image-20240201210723229](https://blog-resources.this0.com/image/202405082037526.png?x-oss-process=style/this0-blog)
