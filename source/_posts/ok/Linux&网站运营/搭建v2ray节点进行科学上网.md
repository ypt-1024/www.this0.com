### 1 前言

v2ray是一种网络传输工具，通过使用V2ray节点，我们可以科学上网（翻墙）学习国外先进科学技术，而这只是v2ray的一点点小功能，本篇也只介绍这一个主题，详细了解可以参考[V2Ray官方文档](https://toutyrater.github.io/)。

### 2 搭建前准备

- 一台国外vps

如果只是单纯想科学上网，直接购买节点更省时省力，本文只分享技术,`不引流、不分享、不售卖节点`。

### 3 服务器搭建v2ray节点

#### 1 节点搭建

运行搭建脚本，脚本guithub地址：https://github.com/233boy/v2ray/wiki/V2Ray%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC

```
bash <(wget -qO- -o- https://git.io/v2ray.sh)
```

搭建完成出现的`蓝色链接`，就是v2ray节点链接，复制下来，后面有用。

（输入命令 v2ray,然后按3可以再查看）

![屏幕截图_20240325_224820](https://blog-resources.this0.com/image/202403252254033.png?x-oss-process=style/this0-blog)

搭建完还不能访问，还要放行节点的端口

#### 2 端口放行

外置防火墙在云服务器厂商处配置，这里只讲内置防火墙，对防火墙不了解的，请参考站内文章，《服务器防火墙配置》//TODO

##### 1 打开节点端口

```
sudo firewall-cmd --zone=public --add-port=节点端口号/tcp --permanent
```

##### 2 重启服务生效

```
sudo firewall-cmd --reload
```

##### 3 验证

查看开放的端口

```
firewall-cmd --list-ports --zone=public
```

41272就是我搭建的节点的端口，已经放行了

![屏幕截图_20240325_225833](https://blog-resources.this0.com/image/202403252259023.png?x-oss-process=style/this0-blog)

也可以通过工具网站检测

比如，https://tcp.ping.pe/

或者https://ping.sx/check-port

打开网站输入ip:端口号，点击Go/Check,出现绿色就是正常放行

### 4 节点使用

我使用的archlinux系统，大同小异

有很多客户端可供选择，我使用下来，windows下的v2rayN，安卓下的Clash体验较好,这两种图形化客户端很好上手，直接导入节点就能使用，所以不多讲了。

下面讲讲我在linux下用的客户端——[v2raya](https://v2raya.org/)。

#### 1 安装v2ray

```
yay -S v2ray
```

#### 2 启动&自启动v2ray

```
systemctl enable --now v2ray
```

#### 3 安装可视化客户端v2raya

```
yay -S v2raya
```

#### 4 启动&自启动v2ray

```
systemctl enable --now v2raya
```

#### 5 导入&使用节点

启动完之后，打开http://127.0.0.1:2017/

进入v2raya的控制台

第一步，点击导入输入上文保存的节点链接,导入节点

![屏幕截图_20240326_023606](https://blog-resources.this0.com/image/202403260238483.png?x-oss-process=style/this0-blog)

第二步，启用节点，依此点击server——勾选节点——启动（点击之后变正在运行）——设置——透明代理/系统代理——启用大陆白名单模式

![屏幕截图_20240326_024247](https://blog-resources.this0.com/image/202403260248427.png?x-oss-process=style/this0-blog)

![屏幕截图_20240326_025056](https://blog-resources.this0.com/image/202403260251407.png?x-oss-process=style/this0-blog)

至此开启了浏览器代理，能正常打开www.google.com等网站了，更多高级功能参考[v2raya官网](https://v2raya.org/)

