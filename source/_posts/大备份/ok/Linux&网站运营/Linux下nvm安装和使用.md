---
title: nvm安装和使用
tags:
  - 前端
  - node
categories:
  - 环境安装
  - nvm安装
mathjax: true
description: linux安装nvm和简单使用教程
abbrlink: c2bdb00f
date: 2023-08-14 16:29:02
---

### 1. nvm官网

nvm gIthub地址： https://github.com/nvm-sh/nvm

### 2. 下载并解压nvm到自定义安装目录

#### 1 创建nvm安装目录

```
sudo mkdir -p /opt/.nvm
```

#### 2 下载(0.39.7版本)

```
sudo wget https://github.com/nvm-sh/nvm/archive/refs/tags/v0.39.7.tar.gz	
```

#### 3 解压文件到 /opt/.nvm

```
 sudo tar -zxvf v0.39.7.tar.gz -C /opt/.nvm
```

#### 4 确保权限

$USER是用户名，使用whomai查看

```
sudo chown -R $USER:$USER /opt/.nvm
```

### 3. 配置环境变量

#### 1 创建自定义配置文件

我的环境变量配置文件，是在/etc/profile.d/目录下，新建了一个配置文件my_env.sh

```
cd /etc/profile.d
touch my_env.sh
```

不建议直接修改配置文件来添加环境变量，这里使用将nvm配置文件my_env.sh包含到~/.zshrc的方法实现

（！！我使用的zsh,不是bash！！）

```bash
sudo vim ~/.zshrc
```

在~/.zshrc的末尾，添加如下语句：

```
#nvm环境变量
if [ -f /etc/profile.d/my_env.sh ]; then
    . /etc/profile.d/my_env.sh
fi
```

#### 2 nvm环境变量配置

```
sudo vim /etc/profile.d/my_env.sh
```

添加以下内容

```bash
export NVM_DIR="/opt/.nvm/nvm-0.39.7"  
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
```

### 4. 使环境配置生效

```bash
source /etc/profile.d/my_env.sh    
```

### 5. 验证nvm是否安装成功

```bash
nvm -v
```

### 6. nvm常用操作：

```
#nvm常用命令
nvm install 16.14.0			// 安装
nvm uninstall 16.14.0     	// 卸载
nvm use 16.14.0           	// 切换 
nvm ls                   	// 查看目前已安装的 node 及当前所使用的 node
nvm ls-remote            	// 查看目前线上所能安装的所有 node 版本
```

### 7. 安装node，自动安装npm包管理工具

### 8. 换源

换成国内淘宝源

```
npm config set registry https://registry.npmmirror.com
```

### 9. 配置全局依赖存储目录

```
npm config set prefix /home/ypt/SDE/GlobalNodeModules
```

查看是否生效

```
npm config get prefix
```
