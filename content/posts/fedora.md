---
title: "fedora"
date: "2022-10-21"
tags: ["Linux"]
summary: "最喜欢的系统（虽然它为了避开专利，下一些驱动比较烦人吧"
---

自己下载包安装的第三方软件结构大概是这样：

- 可执行文件都在 `/usr/local/bin`
- 依赖项在 `/usr/local/lib` 还有 `/usr/local/lib64/`
- 软件的资源文件则是在 `/opt`
- 桌面快捷方式在 `/usr/share/applications`

当然，也有不老实的软件，`bin`文件夹里只放了软链接

# 网易云音乐的~~安装~~复制粘贴

　　解压了网易云音乐的deb包，也是上述结构，然后就手动安装上了。`ar x netease*.deb` 之后，直接双击解压，不愧是我，用什么 `tar -xzvf net*`，最后就是对应位置，疯狂地复制粘贴。

### 缺少 libnsl.so.1

`sudo dnf install libnsl`

### 图标不显示

```shell
vim /usr/share/applications/netease-cloud-music.desktop
Icon=/usr/share/icons/hicolor/scalable/apps/netease-cloud-music.svg
```

### 字体过小

　　使用编辑器修改 `/opt/netease/netease-cloud-music/netease-cloud-music.bash` 的内容，在最后一行中添加 `--force-device-scale-factor=xxx` ，xxx 是字体放大的倍数。修改过后如下：

```shell
HERE="$(dirname "$(readlink -f "${0}")")"
export LD_LIBRARY_PATH="${HERE}"/libs
export QT_PLUGIN_PATH="${HERE}"/plugins 
export QT_QPA_PLATFORM_PLUGIN_PATH="${HERE}"/plugins/platforms
cd /usr/lib64/ # 需要一个依赖项，改变了一下目录
exec "${HERE}"/netease-cloud-music --force-device-scale-factor=1.5 $@ # 在这里写
```

还是官方软件香，就马上卸了 [musicbox](https://github.com/darknessomi/musicbox) 和 mpg123。

# qq音乐 deb转rpm

学聪明了

```
sudo dnf install -y alien rpmrebuild
sudo alien -r qqmusic*.deb
sudo rpm -i qqmusic*.rpmr
```

# Appimage包的安装

　　这个就牛了，自带依赖的库，改了权限`chmod a+x`之后就能执行。放到 `/usr/local/bin` 里，就不用再设置环境变量了，放 `/opt` 里不行。

　　写个 .desktop 文件：

```.desktop
[Desktop Entry]
Name=纯纯写作
Exec=/usr/local/bin/PureWriter
Icon=/opt/icon/PureWriter.png
Type=Application
StartupNotify=true
```

# win10虚拟机

1. 查看是否支持虚拟化
`cat /proc/cpuinfo |egrep "vmx|svm"`。`vmx`表示intel的cpu，`svm`是amd的cpu
2. 安装KVM/QEMU
`sudo dnf -y install bridge-utils libvirt virt-install qemu-kvm`
	查看安装进程
`lsmod |grep kvm`
	安装，KVM的工具包
`sudo dnf -y install virt-top libguestfs-tools`
3. 启动`systemctl start libvirtd`
加入开机自启`systemctl enable libvirtd`

4. 安装可视化界面`sudo dnf -y install virt-manager`

# Typora

在 [FZUG源](https://mirrors.tuna.tsinghua.edu.cn/fzug/free/36/x86_64/) 能找到一个旧的beta版，永远不会升级，永远不会过期好耶。所以就没添加源，只是[点击下载](https://mirrors.tuna.tsinghua.edu.cn/fzug/free/36/x86_64/typora-0.10.11-1.fc36.x86_64.rpm)，然后安装。

# 福昕PDF

链接[在这里](https://www.foxit.com/)，当时是看的[这个教程](https://www.linuxcapable.com/how-to-install-foxit-pdf-reader-on-fedora-34-35/)安装的。

能插入书签和划线，比自带的那个好用多了。
