---
2/24/23:48
---

###  1 安装

sudo pacman -S docker

### 2 启动及自启

```
sudo systemctl start docker
sudo systemctl enable docker
```

### 3 添加用户到docker组

```
#添加用户到docker组
sudo usermod -aG docker ypt
#临时切换docker组
newgrp docker
```

完成
