### 第一步：安装gcc和g++

```
yum -y install gcc
yum -y install gcc-c++
```

### 第二步：安装必要的一些系统工具

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 第三步：添加软件源信息

```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 第四步：更新yum软件包索引并安装Docker-CE

```bash
yum makecache fast

yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 第五步：开启Docker服务

```
systemctl start docker
systemctl enable docker
```

### 第六步：测试是否安装成功

```
docker -v
```

### 第七步：配置镜像加速器

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://ldu6wrsf.mirror.aliyuncs.com"]
}
EOF
```

重启服务

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

