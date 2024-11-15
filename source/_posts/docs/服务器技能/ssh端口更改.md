---
title: 服务器的SSH连接端口更改
tags:
  - SSH
categories:
  - 服务器
description: 两步改ssh连接端口
abbrlink: 2adc8523
date: 2023-10-26 05:36:00
update: 2024-10-31 05:36:00
---
## 两步改ssh连接端口

### 1 编辑ssh配置文件，自定义连接端口

```
vim /etc/ssh/sshd_config	
```

找到port那一行，默认是22端口

![屏幕截图_20240326_025915](https://blog-resources.this0.com/image/202403260259388.png?x-oss-process=style/this0-blog)

比如，想改成8081端口，就取消注释，改端口数字

![屏幕截图_20240326_030208](https://blog-resources.this0.com/image/202403260302239.png?x-oss-process=style/this0-blog)

### 2 重启服务生效

```
systemctl restart sshd.service
```

## 注意

`防火墙端口自行放行!!！`

站内有防火墙配置文章

```bash
sudo firewall-cmd --zone=public --add-port=更改的端口/tcp --permanent

#重启防火墙
sudo firewall-cmd --reload
```

