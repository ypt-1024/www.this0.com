### 1 安装

```
sudo pacman -S mysql 
```

### 2 初始化

初始化的时候会创建一个叫mysql的用户、一个mysql组、mysql的root用户、生成root用户初始密码。不要改路径，改了直接执行会报错。

注意，务必记下初始密码，后面用。

```
 sudo mysqld --initialize --user=mysql --basedir=/usr --datadir=/var/lib/mysql 
```

密码在倒数第二行

---

2024-03-27T19:34:24.585990Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost
: `Ssfpyoyt5-t<`
2024-03-27T19:34:27.150507Z 0 [System] [MY-015018] [Server] MySQL Server Initialization - end.

---

没记下来就执行卸载命令卸载mysql重新初始化，或者查看mysqld.log

```
cat /安装路径下/mysqld.log
```

### 3 设置mysql开机自启

```
#启动及自启
systemctl enable --now mysqld
```

### 4 修改密码

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
```

### 5 可视化连接工具

推荐海狸，免费，能连接N多数据库

海狸官网：https://dbeaver.io/

### 6 mysql安全

执行mysql安全配置

```
mysql_secure_installation
```

选项内容包含

- 选择密码验证策略强度，建议生产环境选择高强度的密码验证策略。

- 设置 MySQL 密码

- 移除匿名用户。

- 设置是否禁止远程连接 MySQL

- 删除 test 库及对 test 库的访问权限。

- 重新加载授权表。

//TODO服务器上可不能这样安装完就了事，现在这样，数据库就是在网络上裸奔，数据库安全见我另一篇文章。
