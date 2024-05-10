### 1 下载

maven官方下载地址：https://maven.apache.org/download.cgi

### 2 解压到指定目录

```
sudo tar -zxvf apache-maven-3.9.6-bin.tar.gz -C /opt/
```

### 3 环境变量配置(可选)

```
vim /etc/profile.d/my_env.sh  
```

添加内容：

```
#maven
export MAVEN_HOME=/opt/apache-maven-3.9.6/
export PATH=$PATH:${MAVEN_HOME}/bin
```

### 4 验证

![屏幕截图_20240327_193416](https://blog-resources.this0.com/image/202403271934307.png?x-oss-process=style/this0-blog)

### 5 maven配置文件修改

#### 1 修改maven的本地仓库（可选）

配置文件在

/maven安装路径/apache-maven-3.9.6/conf/settings.xml

打开settings.xml，在配置文件中搜索以下内容

<localRepository>/path/to/local/repo</localRepository>

会发现这段代码在注释中，作用是指定maven本地仓库的配置，复制黏贴到注释外生效。

比如，我想把本地仓库放在/home/ypt/SDE/maven/repo/

就把代码复制粘贴到下一行，修改成：

```xml
<localRepository>/home/ypt/SDE/maven/repo/</localRepository>
```

#### 2 中央仓库修改(可选)

修改maven的远程仓库地址，这里用阿里云 (下载快)，不做修改，直接用默认的国外仓库也可以

```xml
<mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

