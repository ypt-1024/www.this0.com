## linux服务器安装mysql——自定义安装路径

### 1 自定义安装路径

博主安装在服务器的 /usr/local/this0/mysql 路径下

```
mkdir /usr/local/this0/mysql
cd /usr/local/this0/mysql
```

### 2 上传安装包

上传安装包到安装路径，站长用的xftp上传

### 3 解压安装包

```
#安装包名可以用tap键提示补全
tar -zxvf mysql-8.0.34-linux-glibc2.28-x86_64.tar.gz 
#改个短点的名
mv mysql-8.0.34-linux-glibc2.28-x86_64 mysql-8.0.34
```

### 4 添加mysql用户组和用户

```
 groupadd mysql
 useradd -r -g mysql mysql
```

### 5 自定义数据存放目录

```
mkdir /usr/local/this0/mysql/data
```

### 6 授权

```
#授予刚创建的用户，安装路径权限
chown -R mysql.mysql /usr/local/this0/mysql
```

### 7 配置文件

```
#编辑配置文件
vim /etc/my.cnf
```

```
[mysqld]
## 基础位置
basedir = /usr/local/mysql/mysql-8.0.34
## 数据存放位置
datadir = /usr/local/mysql/data
## 端口
port = 10241
  
socket = /tmp/mysql.sock
## 字符集
character-set-server=utf8
  
log-error = /usr/local/mysql/data/mysqld.log
pid-file = /usr/local/mysql/data/mysqld.pid
```

### 8 初始化mysql

```
/usr/local/this0/mysql/mysql-8.0.34/bin/mysqld --initialize --user=mysql --basedir=/usr/local/this0/mysql/mysql-8.0.34 --datadir=/usr/local/this0/mysql/data/
```

### 9 查看mysql密码

```
cat /usr/local/this0/mysql/data/mysqld.log
```

### 10 启动mysql

```
/usr/local/this0/mysql/mysql-8.0.34/support-files/mysql.server start
```

### 11 将mysql的bin目录添加到系统变量

```
echo 'export PATH="$PATH:/usr/local/this0/mysql/mysql-8.0.34/bin"' >>  /etc/profile 
```

### 12 使配置文件立即生效

```
#或者重启一下也能生效
source /etc/profile
```

### 13 连接测试

```
 mysql -u root -p
 #输入密码时没有提示
```

### 14.设置mysql开机自启

#### 复制服务到自启动路径下

```
cp /usr/local/mysql/mysql-8.0.34/support-files/mysql.server /etc/init.d/mysqld
```

#### 显示服务列表

```
chkconfig --list
```

#### 添加服务

```
chkconfig --add mysqld
```

#### 重新查看显示服务列表

```
chkconfig --list
```

#### 如果是关闭的话，使用下面命令将其开启

```
chkconfig --level 345 mysqld on
```

![img](https://blog-resources.this0.com/image/202405061643510.png?x-oss-process=style/this0-blog)



![img](https://blog-resources.this0.com/image/202405061643466.png?x-oss-process=style/this0-blog)

### 15 修改密码

```
mysql -u root -p
```

1. 修改root密码

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'your_strong_password';
```

2. 创建新用户：

```
CREATE USER 'abcd'@'%' IDENTIFIED BY '123456';
```

3. 授予用户全部权限：

```
GRANT ALL PRIVILEGES ON *.* TO 'abcd'@'%';
```

4. 刷新权限以应用更改：

```
FLUSH PRIVILEGES;
```

### 16.安全设置

1. 删除测试数据库：MySQL安装后，可能会自带一些测试数据库，如`test`，`mysql`等。这些数据库对于生产环境没有必要，可以将其删除。

   ```
   DROP DATABASE IF EXISTS test;
   ```

2. 移除匿名用户：MySQL默认创建了一个允许匿名用户访问的账户，为了安全起见，应该将其删除。

   ```
   DELETE FROM mysql.user WHERE User='';
   ```

