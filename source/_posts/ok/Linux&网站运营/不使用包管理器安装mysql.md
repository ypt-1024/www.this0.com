---
abbrlink: '0'
---
## linux服务器解压安装mysql

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

### 14 拷贝到开机自启服务

```
cp /usr/local/this0/mysql/mysql-8.0.34/support-files/mysql.server /etc/init.d/mysqld
```

