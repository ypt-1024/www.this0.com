---
title: nvm安装和使用
tags:
  - Redis
categories:
  - Redis
mathjax: true
description: linux安装nvm和简单使用教程
abbrlink: c2bdb00f
date: 2023-08-05 16:29:02
---
### 1 安装

```
yay -S redis
```

### 2 自启

```
systemctl enable --now redis
```

### 3 修改配置文件

```
vim /etc/redis/redis.conf
```

配置文件修改4处：

1 后端启动

daemonize yes

2 保护模式

protected-mode no

3 远程连接

注释掉

bind 127.0.0.1 -::1

4 redis访问密码

requirepass redis
