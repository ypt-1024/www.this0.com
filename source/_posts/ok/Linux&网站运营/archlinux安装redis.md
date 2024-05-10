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
