---
title: nvm安装和使用
tags:
  - Mysql
categories:
  - Mysql
mathjax: true
description: linux安装nvm和简单使用教程
abbrlink: c2bdb00f
date: 2023-08-15 16:29:02
---
### 1 安装

```
sudo pacman -S mysql 
```

### 2 初始化

初始化的时候会创建一个叫mysql的用户、一个mysql组、mysql的root用户、生成root用户初始密码。不要改路径，改了直接执行会报错。

注意，务必记下初始密码，后面用。

```
 sudo mysqld --initialize --user=mysql --basedir=/usr --datadir=/var/lib/mysql 
```

密码在倒数第二行

---

2024-03-27T19:34:24.585990Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost
: `Ssfpyoyt5-t<`
2024-03-27T19:34:27.150507Z 0 [System] [MY-015018] [Server] MySQL Server Initialization - end.

---

没记下来就执行卸载命令卸载mysql重新初始化，或者查看mysqld.log

```
cat /安装路径下/mysqld.log
```

### 3 设置mysql开机自启

```
#启动及自启
systemctl enable --now mysqld
```

### 4 修改密码

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
```

### 5 可视化连接工具

推荐海狸，免费，能连接N多数据库

海狸官网：https://dbeaver.io/

### 6 mysql安全

本地安装，不用考虑安全问题

//TODO服务器上可不能这样安装完就了事，这样数据库相当于在网络上裸奔，服务器安装mysql涉及到的数据库安全部分，见站内文章。
