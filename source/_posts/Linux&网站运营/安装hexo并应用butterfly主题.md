---
title: 安装hexo并应用butterfly主题
tags:
  - 网站说明文档
  - 环境搭建
categories:
  - 网站说明文档
description: 本文介绍了如何安装hexo并应用butterfly主题
abbrlink: 268d8d1e
date: 2023-10-26 05:36:00
update: 2025-3-31 10:00:00
---
### 1. 安装前提

安装 Hexo 相当简单，只需要先安装下列应用程序即可：

- [Node.js](http://nodejs.org/) (本次使用 Node.js 22.14.0)
- [Git](http://git-scm.com/)

### 2. 安装Hexo

所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。

```
npm install -g hexo-cli

#验证是否安装成功。
hexo -v

#hexo-cli: 4.3.2
```

### 3. 安装插件

如果你没有 pug 以及 stylus 的渲染器，请下载安装，否则运行时不能正常显示画面：

```
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

### 4. 初始化hexo项目

```
hexo init blog
#blog替换成你自己取的项目名
```

### 5. 运行hexo

```
hexo cl; hexo g; hexo s -p 80
```

#分别代表：清理静态文件；生成静态文件；启动服务 (-p是以指定端口启动)

### 6. 访问

本地访问http://localhost/

### 7. 安装主题(可选)

以butterfly主题为例

> 可选git安装或者npm安装，建议用git安装，npm安装还需要从node_modules里把主题文件移出来。

#### 1. 通过git安装主题

```
git clone -b master https://gitee.com/immyw/hexo-theme-butterfly.git themes/butterfly
```

#### 2. npm安装（二选一）

```
npm install hexo-theme-butterfly
```

#### 3. 应用主题

修改 Hexo 根目录下的 _config.yml，把主题改为 butterfly

```yml
theme: butterfly #大概在第99行
```

#### 4. 升级主题

升级前将hexo-theme-butterfly文件夹备份，npm更新会直接覆盖成新的包

```bash
npm update hexo-theme-butterfly
```

#### 5. 优化设置

为了减少升级主题后带来的不便，请使用以下方法（建议，可以不做）。

在 hexo 的根目录创建一个文件 _config.butterfly.yml，并把主题目录的 _config.yml 内容复制到 _config.butterfly.yml 去。( 注意: 复制的是主题的 _config.yml ，而不是 hexo 的 _config.yml)

#### 6. 注意事项

1. 不要把主题目录的 _config.yml 删掉

2. 以后只需要在 _config.butterfly.yml 进行配置就行。
   如果使用了 _config.butterfly.yml， 配置主题的 _config.yml 将不会有效果。

3. Hexo会自动合并主题中的 _config.yml 和 _config.butterfly.yml 里的配置，如果存在同名配置，会使用 _config.butterfly.yml 的配置，其优先度较高。

#### 7. 主题配置

参考 [Butterfly 文档(二) 主题页面](https://butterfly.js.org/posts/dc584b87/) 和 [Butterfly 文档(三) 主题配置](https://butterfly.js.org/posts/4aa8abbe/) 进行自定义