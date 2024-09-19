---
abbrlink: '0'
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

注意：防火墙端口自行配置