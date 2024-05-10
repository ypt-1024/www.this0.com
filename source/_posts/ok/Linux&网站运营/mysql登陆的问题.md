### （1）远程连接问题

在用SQLyog或Navicat中配置远程连接MySQL数据库时遇到如下报错信息，这是由于MySQL默认不支持远

程连接。

![image-20220625191443737](https://blog-resources.this0.com/image/image-20220625191443737.png)



查看系统数据库MySQL中的user表：

```sql
USE mysql;
SELECT Host,User FROM user;
```

![image-20220625231126326](https://blog-resources.this0.com/image/image-20220625231126326.png)

可以看到root用户的当前主机配置信息为localhost。**修改Host为通配符%**

Host列指定了允许用户登录所使用的IP：

- `Host=localhost`，表示只能通过本机客户端去访问。

- `Host=%` ，表示所有IP都有连接权限。

```sql
UPDATE user SET Host = '%' WHERE User ='root';
FLUSH PRIVILEGES; -- Host修改完成后记得执行FLUSH PRIVILEGES使配置立即生效：
```

> 注意：在生产环境下不能为了省事将host设置为%，这样做会存在安全问题，可以设置为生产环境IP。

### （2）使用SQLyog连接

![image-20230613221609053](https://blog-resources.this0.com/image/image-20230613221609053.png)

出现这个原因是MySQL 8 之前的版本中加密规则是mysql_native_password，而在MySQL 8之后，加密规则是caching_sha2_password。

解决方案有两种，一种是升级SQLyog和Navicat（因此，新版SQLyog不会出现此问题），另一种是把MySQL用户登录密码加密规则还原成mysql_native_password。


**解决方法：**Linux下 mysql -uroot -p 登录你的 MySQL 数据库，然后 执行这条SQL：

```sql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
```

然后再重新配置SQLyog的连接，重新填写密码，则可连接成功了。 
