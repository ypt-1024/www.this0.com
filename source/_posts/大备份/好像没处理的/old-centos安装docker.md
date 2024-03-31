环境安装：
yum -y install gcc-c++

第一步：安装必要的一些系统工具
yum install -y yum-utils device-mapper-persistent-data lvm2

第二步：添加软件源信息
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

第三步：更新并安装Docker-CE
yum makecache fast
yum -y install docker-ce


mv /usr/bin/systemctl /usr/bin/systemctl.old
curl https://raw.githubusercontent.com/gdraheim/docker-systemctl-replacement/master/files/docker/systemctl.py > /usr/bin/systemctl
chmod +x /usr/bin/systemctl

第四步：开启Docker服务
systemctl start docker
systemctl enable docker

第五步：测试是否安装成功
docker -v

第六步：配置镜像加速器

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://ldu6wrsf.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker