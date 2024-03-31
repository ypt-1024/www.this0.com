#下载docker包
wget https://download.docker.com/linux/static/stable/x86_64/docker-18.09.9.tgz

#解压
tar zxf docker-18.09.9.tgz

#移动解压后的文件夹到/usr/bin
mv docker/* /usr/bin

#写入docker.service
cat >/usr/lib/systemd/system/docker.service <<EOF
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target
[Service]
Type=notify
ExecStart=/usr/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
LimitNOFILE=infinity
LimitNPROC=infinity
TimeoutStartSec=0
Delegate=yes
KillMode=process
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s
[Install]
WantedBy=multi-user.target
EOF

#启动docker
systemctl start docker

#设置开机自启动
systemctl enable docker

#查看docker版本
docker version

#运行hello-world
#docker run hello-world