---
title: ArchLinux安装
description: 本文介绍使用archinstall脚本安装ArchLinux桌面环境，以及安装完成之后的配置
tags:
  - Linux
  - ArchLinux
  - 开发环境
categories:
  - 工具文档
abbrlink: 2013454d
date: 2023-08-12 19:01:34
update: 2024-06-29 16:00:00
---

> 使用官方提供的安装脚本：archinstall，快速安装ArchLinux，并进行必要配置

### 1. 镜像制作

#### 1 镜像下载

[Arch Linux - Downloads](https://archlinux.org/download/)

#### 2 镜像制作工具下载

[rufus](http://rufus.ie/downloads/)

#### 3 使用Rufus制作启动U盘

> 24年6月安装的，图是以前的图

##### 1 选择镜像文件

##### 2 改分区类型为GPT

<img src="https://blog-resources.this0.com/image/202403232047624.png?x-oss-process=style/this0-blog" alt="image.png" />
图1:改分区类型为GPT

##### 3 dd模式写入镜像

![image.png](https://blog-resources.this0.com/image/202403232047095.png?x-oss-process=style/this0-blog)
图2:dd模式写入镜像

### 2. BIOS设置

#### 1 进入BIOS设置

#### 2 禁用安全模式

#### 3 修改启动顺序，将U盘启动顺序放在第一个

#### 4 按F10 保存重启

### 3. 连接WiFi，使用archinstall安装系统

知道wifi账号和密码，直接station wlan0 connect wifi名称,回车输入wifi密码即可，不能就按下面流程完整走一遍

#### 1 使用无线网络管理工具iwctl，输入iwctl进入

```shell
iwctl
```

下面的操作都是在`iwctl命令内`完成。
 首先，如果不知道你的网络设备名称，

#### 2 列出所有 WiFi 设备

```shell
device list
```

一般无线网卡设备名都是`wlan0`。

#### 3 扫描网络

```shell
station wlan0 scan
```

#### 4 列出所有可用的网络：

```shell
station wlan0 get-networks
```

比如要连接的WiFi叫`CMCC`，要连接到这个网络：

```shell
station wlan0 connect CMCC
```

输入密码后回车，就连接上了WiFi。接着退出iwctl

```shell
exit
```

测试网络是否连通：

```shell
ping baidu.com
```

### 4 使用archinstall安装系统

安装之前先换镜像源，否则使用默认的国外源，下载安装系统很慢，命令

```
reflector --country China --age 72 --sort rate --protocol https --save /etc/pacman.d/mirrorlist
```

#### 1 镜像系统更新

```
pacman -Sy
```

#### 2 更新archinstall脚本

```
pacman -S archinstall
```

#### 3 正式安装

执行安装脚本

```
archinstall
```

##### 1 镜像区域选择

选择中国

##### 2 本地编码设置

改成zh_CN.UTF-8

![image-20240320015818888](https://blog-resources.this0.com/image/202403232047089.png?x-oss-process=style/this0-blog)

##### 3 启动器改成grub（可选）

可以不该，linux下讲grub的教程更多，但是默认的启动器更小巧，更合适

![image-20240320015942676](https://blog-resources.this0.com/image/202403232047086.png?x-oss-process=style/this0-blog)

##### 4 系统配置文件

桌面版本我选择的kde，根据个人喜好选择，驱动根据自己显卡型号来装，不知道就全装。

![image-20240320020112001](https://blog-resources.this0.com/image/202403232047083.png?x-oss-process=style/this0-blog)

##### 5 音频播放器

![image-20240320020242882](https://blog-resources.this0.com/image/202403232047092.png?x-oss-process=style/this0-blog)

```
#推荐使用pipewire，可以用下面命令安装pipewire
sudo pacman -Sy pipewire wireplumber
```

##### 6 网络管理器

使用KDE自带的网络管理器

![image-20240320020318475](https://blog-resources.this0.com/image/202403232047666.png?x-oss-process=style/this0-blog)

##### 7 选择install选项执行安装脚本

安装完后reboot重启

### 5. 基本设置

#### 1 安装常用字体

```bash
sudo pacman -S ttf-hannom ttf-fira-code noto-fonts-extra noto-fonts-emoji noto-fonts-cjk adobe-source-code-pro-fonts adobe-source-sans-fonts adobe-source-serif-fonts adobe-source-han-sans-cn-fonts adobe-source-han-sans-hk-fonts adobe-source-han-sans-tw-fonts adobe-source-han-serif-cn-fonts wqy-zenhei wqy-microhei
```

#### 2 开启sshd服务，方便远程访问(可选)

```bash
systemctl start sshd

systemctl enable sshd
```

#### 3 ssh连接报错的解决方法

使用命令行连接过/变更密码导致ssh连接报错的解决方法：

进入  C:\Users\用户名\.ssh，删除连接密钥

#### 4 ssh连接root用户(可选)

> 也可以通过ssh连接到普通用户，提权到root用户，不用改配置文件

配置文件

```bash
vim	/etc/ssh/sshd_config
```

将PermitRootLogin no 改为 PermitRootLogin yes

#### 5. 显卡驱动(可选)

> archinstall脚本里操作过了，可以省略
>
> 因为很重要，所以还是单独出一节

##### 1 安装显卡驱动

```bash
pacman -S xf86-video-intel
                                            (Intel核心显卡驱动，用Intel核显就装，否则不用装)
pacman -S mesa nvidia(-lts) nvidia-settings nvidia-dkms nvidia-utils nvidia-prime
                                            (nvidia显卡驱动，用nvidia显卡就装，否则不用装)
pacman -S xf86-video-amdgpu
                                              (AMD显卡驱动，用amd显卡的就装)
```

nvidia-dkms 与 nvidia-lts 不兼容，如果装lts驱动的话无需安装dkms 。

##### 2 例

我的笔记本，AMD核显以及3060显卡，所以安装后两个。

```bash
pacman -S mesa nvidia-lts nvidia-settings  nvidia-utils nvidia-prime xf86-video-amdgpu
```

#### 6 蓝牙

现在基本都默认启用蓝牙功能，不再需要单独安装

```bash
#安装蓝牙
sudo pacman -S bluez bluez-utils
#启动
sudo systemctl start bluetooth
sudo systemctl enable bluetooth
```

#### 7 grup设置

将等待时间设置为0

```
sudo vim /etc/default/grub
```

改成 GRUB_TIMEOUT=0
使生效

```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

![屏幕截图_20240323_183124](https://blog-resources.this0.com/image/202403232047995.png?x-oss-process=style/this0-blog)

#### 8 解压乱码问题

第一步，直接取消勾选Libzip插件

![image-20240416020207018](https://blog-resources.this0.com/image/202404160204026.png?x-oss-process=style/this0-blog)

第二步，安装p7zip-natspec或者替换p7zip为p7zip-natspec

```
yay -S p7zip-natspec
```

#### 9 访问ntfs格式硬盘

```
sudo pacman -S ntfs-3g
```

### 6. 添加存储库

#### 6.1 ArchLinuxCN

##### 1 编辑存储库文件

```bash
sudo vim /etc/pacman.conf
```

在末尾添加以下内容

```
[archlinuxcn]
SigLevel = Optional TrustAll     # 需要加上这个配置，否则会出现问题
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

![image-20240319130026829](https://blog-resources.this0.com/image/202403232047739.png?x-oss-process=style/this0-blog)

这是中科大的源，你也可以选择清华、阿里等

##### 2 更新GPG密钥

```bash
sudo pacman -Sy archlinuxcn-keyring
```

#### 6.2 Mutilab

在安装的时候已经添加了，如果没有在安装过程添加，在存储库文件中找到，然后放开注释即可

### 7. Archlinux常用软件安装

#### 1 git

```bash
sudo pacman -S git
```

#### 2 AUR工具(yay)

> 也可以试试paru，这里我使用yay

推荐方法：先按上文添加archlinuxcn存储库，然后直接

```
pacman -S yay
```

不然就编译安装,会使用github下载安装文件，因为国内网络原因，容易编译失败

```
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```

#### 3 输入法

##### 1. 安装 fcitx5

```bash
sudo pacman -S fcitx5-im fcitx5-qt fcitx5-gtk fcitx5-chinese-addons fcitx5-pinyin-zhwiki
```

##### 2. 配置输入法环境

```
sudo vim /etc/environment
```

加入以下内容：

```bash
XMODIFIERS=@im=fcitx
INPUT_METHOD=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=ibus
#虽然提示不用加下面两行，但是不加有的应用不能正常使用输入法，一定要加上
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx 
```

重启后即可生效

#### 4 flatpak

flatpak可以搭配kde的软件商店Discover使用，实现可视化软件管理

##### 1 安装flatpak

##### 2 换国内源(上交大)

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak remote-modify flathub --url=https://mirror.sjtu.edu.cn/flathub
```

安装完后，大部分软件都可以从Discover中下载,但，一些软件的专有版本，还是需要参考wiki，下载特定版本，比如从flatpak里下载的vscode，集成不了zsh。

#### 5 Vscode

```
yay -S visual-studio-code-insiders-bin
```

#### 6 fuse2

fuse2环境，解决双击appimage文件执行不了的问题

```
sudo pacman -S fuse2
```

#### 7 截图工具 spectacle

```
sudo pacman  -S spectacle
```

#### 8 chrome

```
yay -S google-chrome
```

#### 9 百度网盘

2024年3月，更新KDE6.0.2之后有闪退bug,没解决。

```
yay -S baidunetdisk
```

#### 10 WPS

问题比较多，见[wiki](https://wiki.archlinuxcn.org/wiki/WPS_Office?rdfrom=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%3Ftitle%3DWPS_Office_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29%26redirect%3Dno)

安装 WPS 需要的符号字体：[ttf-wps-fonts](https://aur.archlinux.org/packages/ttf-wps-fonts/)

```
yay -S ttf-wps-fonts
```

#### 11 zsh & oh-my-zsh

##### 1 安装zsh

```
yay -S  zsh
```

##### 2 安装oh-my-zsh

curl安装(Gitee)

```
sh -c "$(curl -fsSL https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"
```

##### 3 改shell

安装完接受不接受都可以，因为后面要自己改命令行工具

![屏幕截图_20240322_182751](https://blog-resources.this0.com/image/202403232047850.png?x-oss-process=style/this0-blog)

这样改：

![屏幕截图_20240322_183404](https://blog-resources.this0.com/image/202403232047872.png?x-oss-process=style/this0-blog)

- 修改配置方案
- 新建配置方案
- 命令填zsh的路径，完成

##### 4 主题配置

配置文件在

```
cat ~/.zshrc
```

主题在这里[浏览](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)

##### 5 插件

命令行用得少，为就不安插件了

#### 12 neofetch

系统状态工具 neofetch

```
sudo pacman -S neofetch
```

![屏幕截图_20240324_214200](https://blog-resources.this0.com/image/202403242146232.png?x-oss-process=style/this0-blog)

#### 13 微信

24年3月，微信 Linux 原生版重构

```
yay -S wechat-universal-bwrap
```

