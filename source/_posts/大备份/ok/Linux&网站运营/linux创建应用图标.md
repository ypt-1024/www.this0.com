### 1 创建桌面图标：

以idea为例

```
vim idea.desktop
```

### 2 编辑图标

复制粘贴以下内容

```
#每个desktop文件都已这个标签开始，说明这是一个Desktop Entry 文件.
[Desktop Entry]

#注明在菜单栏中显示的类别(可选)
Categories=Application

#标明Desktop Entry的版本(可选).
Version=1.0

#程序的执行命令(必选),可以带参数运行
Exec=/home/yupengtao/SDE/idea-IU-212.4746.92/bin/idea.sh

#设置快捷方式的图标(可选).
Icon=/home/yupengtao/SDE/idea-IU-212.4746.92/bin/idea.svg

#程序名称(必须)
Name=idea

#desktop的类型(必选),常见值有“Application”和“Link”.
Type=Application

#是否在终端中运行(可选),当Type为Application,此项有效.
Terminal=false
```

### 3 给执行权限

```
chmod 744 idea.desktop
```

