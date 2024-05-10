### linux通用安装JDK

#### 1 查询并卸载系统中自带的JDK，

以centos7为例，使用自己操作系统的包管理命令

```纯文本
#查询
rpm -qa | grep jdk
#卸载
rpm -e --nodeps jdk的rpm软件包的名字
```

#### 2 将jdk-xxx_bin.tar.gz上传到服务器

以/opt目录为例

官方下载地址：https://www.oracle.com/java/technologies/downloads/

#### 3 解压jdk压缩包

```纯文本
tar -zxvf jdk压缩包名称  -C /opt/
```

#### 4 环境变量配置

##### 1 在自定义配置文件中配置环境变量

(不建议直接修改/etc/profile)

//TODO如何自定义配置文件，参考3.1小节

```
vim /etc/profile.d/my_env.sh
```

添加以下内容

```纯文本
#jdk环境变量
JAVA_HOME=/opt/jdk-22	#安装目录
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME
```

##### 2 没有自定义配置文件

```
vim /etc/profile
```

#### 5 使生效

执行

```
source /etc/profile.d/my\_env.sh
```

使配置立即生效，没有自定义配置文件直接：

```
source /etc/profile
```

重启或者注销用户都可以使环境变量生效 
