---
title: 服务器（centos8）安装docker
tags:
  - Docker
  - 服务器运维
categories:
  - 服务器运维
abbrlink: 2a40ac23
date: 2024-02-01 08:13:19
description:
---

## 服务器（centos8）安装docker

> 2024年2月测试通过

### 1 下载docker包

```
wget https://download.docker.com/linux/static/stable/x86_64/docker-25.0.1.tgz
```



### 2 解压

```
tar zxf docker-25.0.1.tgz
```



### 3 移动解压后的文件夹到/usr/bin

```
mv docker/* /usr/bin
```



### 4 写入docker.service

```
cat >/usr/lib/systemd/system/docker.service <<EOF
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target
[Service]
Type=notify
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
LimitNOFILE=infinity
LimitNPROC=infinity
TimeoutStartSec=0
Delegate=yes
KillMode=process
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s
[Install]
WantedBy=multi-user.target
EOF
```



### 5 启动docker

```
systemctl start docker
```



### 6 设置开机自启动

```
systemctl enable docker
```



### 7 查看docker版本

```
docker version
```



### 8 安装成功界面

![image-20240201081805985](http://cdn.this0.com/blog/img/image-20240201081805985.png)
