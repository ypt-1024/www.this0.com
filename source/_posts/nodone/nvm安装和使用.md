---
title: nvm安装和使用
tags:
  - 网站说明文档
  - 环境搭建
categories:
  - 环境搭建
description: 本文介绍nvm安装和使用
abbrlink: 2023825a
date: 2023-10-26 05:36:00
---

# nvm安装和使用

## 1. 下载nvm安装包

​	[网站同款linux安装包下载](https://github.com/nvm-sh/nvm/archive/refs/tags/v0.39.4.tar.gz)

## 2. 解压nvm到指定目录

```bash
#新建服务器nvm地址
mkdir /root/.nvm
#解压文件 到 root/.nvm
tar -zxvf nvm-0.39.4.tar.gz --strip-components 1  -C /root/.nvm
```

## 3. 配置环境变量

```bash
vim ~/.bashrc
//也可进入到相应的 文件下修改文件
```

在~/.bashrc的末尾，添加如下语句：

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

## 4. 使环境配置生效

```bash
source ~/.bashrc
```

## 5. 验证nvm是否安装成功

```bash
nvm -v
```

## 6. node常用操作：

```
#nvm常用命令
nvm install 16.14.0			// 安装
nvm uninstall 16.14.0     	// 卸载
nvm use 16.14.0           	// 切换 
nvm ls                   	// 查看目前已安装的 node 及当前所使用的 node
nvm ls-remote            	// 查看目前线上所能安装的所有 node 版本
```
