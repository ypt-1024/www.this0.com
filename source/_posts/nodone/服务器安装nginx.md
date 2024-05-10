---
title: nginx基本使用
tags:
  - 网站说明文档
  - 环境搭建
categories:
  - 环境搭建
description: 本文介绍了nginx的基本使用
abbrlink: 5f70ef58
date: 2023-10-26 05:36:00
---
# nginx基本使用

## 一、nginx安装和管理

### 1. 安装nginx

sudo yum install nginx

### 2. 设置nginx开机自启

systemctl enable nginx

### 3. 启动nginx

start nginx

### 4. 重启Nginx

systemctl restart nginx

### 5. 查看nginx启动状态

systemctl status nginx

## 二、nginx配置和最佳实践

> 注意事项

如果你的服务器开启了防火墙，则需要同时打开 80（HTTP）和 443（HTTPS）端口

通过下面的命令来打开这两个端口：

```routeros
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```

- 通过以上方式安装的 Nginx，所有相关的配置文件都在 `/etc/nginx/` 目录中。
- Nginx 的主配置文件是 `/etc/nginx/nginx.conf`。
- 为了使 Nginx 配置更易于维护，建议为每个服务（域名）创建一个单独的配置文件。
- 每一个独立的 Nginx 服务配置文件都必须以 `.conf `结尾，并存储在 `/etc/nginx/conf.d` 目录中。您可以根据需求，创建任意多个独立的配置文件。
- 独立的配置文件，建议遵循以下命名约定，比如你的域名是 `this0.com`，那么你的配置文件的应该是这样的 `/etc/nginx/conf.d/this0.com.conf`，如果你在一个服务器中部署多个服务，当然你也可以在文件名中加上 Nginx 转发的端口号，比如 `this0.com.3000.conf`，这样做看起来会更加友好。
- 如果你的配置中有很多重复的代码，那么建议你创建一个 `/etc/nginx/snippets` 文件夹，在这里面存放所有会被复用的代码块，然后在各个需要用到的 Nginx 的配置文件中引用进去，这样可以更方便管理和修改。
- Nginx 日志文件（`access.log` 和 `error.log` ）位于 `/var/log/nginx/` 目录中。建议为每个独立的服务配置不同的访问权限和错误日志文件，这样查找错误时，会更加方便快捷。
- 你可以将要部署的代码文件，存储在任何你想的位置，但是一般推荐存放在下列位置中的其中一个：
  - `/home/<user_name>/<site_name>`
  - `/var/www/<site_name>`
  - `/var/www/html/<site_name>`
  - `/opt/<site_name>`
  - `/usr/share/nginx/html`

## 三、具体配置

#### 第1步：配置this0.conf

```nginx
server {
    listen       89;
    server_name  this0.com;

    location / {
        root blog;
        index index.html;
    }
}
```

复制

> 说明一下，listen后面跟着的89是咱的监听端口 server_name 后填[域名](https://cloud.tencent.com/act/pro/domain-sales?from_column=20065&from=20065) 然后就是location配置，因为我之前把blog文件夹放在外面，所以直接写blog

#### 第2步：引入this0.conf到nginx.conf

   打开`nginx.conf`

   在这个位置添加

![image.png](http://cdn.this0.com/blog/img/6ea3db7d87b74089209a6280525cb531.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000000&Signature=eppfoVPB514d/H3VPQAUIr9fL8I=&x-oss-process=style/cdn.this0)

然后保存