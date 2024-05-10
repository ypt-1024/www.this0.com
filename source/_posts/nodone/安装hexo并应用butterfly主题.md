---
title: 安装hexo并应用butterfly主题
tags:
  - 网站说明文档
  - 环境搭建
categories:
  - 网站说明文档
description: 本文介绍了如何在hexo基础上，安装butterfly主题。
abbrlink: 268d8d1e
date: 2023-10-26 05:36:00
---
# 安装hexo并应用butterfly主题

## 1. 安装前提

安装 Hexo 相当简单，只需要先安装下列应用程序即可：

- [Node.js](http://nodejs.org/) 在博客内已有安装教程，[点我查看](http://this0.com/post/2023825a.html) (Node.js 版本需不低于 10.13，建议使用 Node.js 12.0 及以上版本)
- [Git](http://git-scm.com/)

## 2. Node.js 版本限制

| Hexo 版本   | 最低版本 (Node.js 版本) | 最高版本 (Node.js 版本) |
| :---------- | :---------------------- | :---------------------- |
| 6.2+        | 12.13.0                 | latest                  |
| 6.0+        | 12.13.0                 | 18.5.0                  |
| 5.0+        | 10.13.0                 | 12.0.0                  |
| 4.1 - 4.2   | 8.10                    | 10.0.0                  |
| 4.0         | 8.6                     | 8.10.0                  |
| 3.3 - 3.9   | 6.9                     | 8.0.0                   |
| 3.2 - 3.3   | 0.12                    | 未知                    |
| 3.0 - 3.1   | 0.10 或 iojs            | 未知                    |
| 0.0.1 - 2.8 | 0.10                    | 未知                    |

## 3. 安装Hexo

所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。

```
npm install -g hexo-cli
#输入hexo -v验证是否安装成功。
```

## 4. 安装插件

如果你没有 pug 以及 stylus 的渲染器，请下载安装：

```
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

## 5. 初始化hexo项目

```
hexo init this0(自己取的项目名)
```

## 6. 运行hexo

hexo cl; hexo g; hexo s -p 80
#分别代表：清理静态文件；生成静态文件；启动服务 (-p是以指定端口启动)

## 7. 访问

本地访问http://localhost/

## 8. 安装butterfly主题

> 可选git安装或者npm安装，建议用git安装，npm安装还需要从node_modules里把主题文件移出来。

### 8.1 git安装

```
git clone -b master https://gitee.com/immyw/hexo-theme-butterfly.git themes/butterfly
```

### 8.2 npm安装

```
npm install hexo-theme-butterfly
```

## 9. 应用主题

修改 Hexo 根目录下的 _config.yml，把主题改为 butterfly

```yml
theme: butterfly #大概在103行
```

## 10. 升级

升级前将hexo-theme-butterfly文件夹备份，npm更新会直接覆盖成新的包

```bash
npm update hexo-theme-butterfly
```

## 11. 优化设置

为了减少升级主题后带来的不便，请使用以下方法（建议，可以不做）。

在 hexo 的根目录创建一个文件 _config.butterfly.yml，并把主题目录的 _config.yml 内容复制到 _config.butterfly.yml 去。( 注意: 复制的是主题的 _config.yml ，而不是 hexo 的 _config.yml)

## 12. 注意事项

1. 不要把主题目录的 _config.yml 删掉

2. 以后只需要在 _config.butterfly.yml 进行配置就行。
   如果使用了 _config.butterfly.yml， 配置主题的 _config.yml 将不会有效果。

3. Hexo会自动合并主题中的 _config.yml 和 _config.butterfly.yml 里的配置，如果存在同名配置，会使用 _config.butterfly.yml 的配置，其优先度较高。

## 13. 使用开源项目（可选）

比如使用我的项目[this0.com](http://www.this0.com),直接复制源码到初始化后的项目，覆盖文件。当然，就不需要初始化这一步了。

