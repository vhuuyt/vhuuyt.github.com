---
title: "Linux折腾大杂烩"
date: "2022-04-13"
weight: 2
tags: ["瞎折腾","Linux"]
summary: "记录一些遇到的问题和解决方法（如果有的话）"
---

# GNOME相关

## ❌⬛➖左移

```bash
gsettings set org.gnome.desktop.wm.preferences button-layout 'close,maximize,minimize:'
gsettings set org.gnome.desktop.wm.preferences button-layout ':close,maximize,minimize'
# 也可使用图形工具修改
sudo apt install dconf-editor
sudo apt install gnome-tweaks
```

## 单击工具栏图标-使软件最小化

```bash
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```

设置回默认值

```bash
gsettings reset org.gnome.shell.extensions.dash-to-dock click-action
```

## 桌面免密自动登录（危险

`/etc/gdm/custom.conf`在最后添加以下内容：

```
[daemon]
TimedLoginEnable=true
TimedLogin=jack
TimedLoginDelay=3
```

简单解释一下：  
TimedLoginEnable=true    允许超时自动登录  
TimedLogin=jack      自动登录的用户为jack  
TimedLoginDelay=3          超时时间为3秒，可不设

# 其他

## sudo 命令免密

方法：在`/etc/sudoers.d/`文件夹中配置sudoers配置

通过命令`sudo visudo`来编辑`/etc/sudoers`,在文件末；也可在`/etc/sudoers.d/`文件夹里新建文件，添加如下内容

```
username ALL=(ALL) NOPASSWD:ALL
```
这样就能让username用户执行任意命令，不需要输入密码。

> 请注意，使用sudoers文件配置免密码访问是不安全的。黑客可以使用你的用户名和密码登录到你的系统并在不需要密码的情况下执行任意命令，请慎重使l用
> `ALL=(ALL:ALL)ALL` 表示给用户或组授权执行任何命令，但是需要输入root用户的密码，而`ALL=(ALL) ALL` 则表示给用户或组授权执行任何命令，但是不需要输入root用户的密码。

## Rime 输入法配置

```
sudo dnf install ibus-rime
ibus-daemon -drx    // restart IBus
```

in IBus Preferences(`ibus-setup`), add "Chinese-Rime" to the input method list.

然后部署再同步，就能自动同步词库了。

`~/.config/ibus/rime/build/ibus_rime.yaml` 文件中，添加了
```
style:
  horizontal: true
```

`~/.config/ibus/rime/default.custom.yaml` 文件中，添加了
```
patch:
  menu:
    page_size: 9
  schema_list:
    - schema: double_pinyin_mspy
```

## 客制化 prompt

```bash
# users 配置 
# ~/.bashrc.d/custom
PS1='\[\e[0;32m\]\w\[\e[0;36m\]¥\[\e[0m\] '

# super user 配置 
# /etc/profile.d/color-ps1.sh
PS1='\[\e[0;31m\]\u \[\e[0;32m\]\W\[\e[0;36m\]#\[\e[0m\] '
```

## 无线键盘

```bash
sudo modprobe usbhid
sudo modprobe hid_generic
sudo reboot
```

## fedora上APU的闭源驱动

```bash
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld
sudo dnf swap mesa-vdpau-drivers mesa-vdpau-drivers-freeworld
```

## vlc不能直接打开安卓手机文件

缺少`gvfs-fuse`组件，安装即可；  
还有，`vlc-plugin-access-extra`，貌似是提供了一些专有格式的解码，我不懂了

## fish配置

```fish
# ~/.config/fish/config.fish
if status is-interactive
	# Commands to run in interactive sessions can go here
	alias q='exit'
	alias sd='sudo shutdown -h now'
	alias cl='clear'
	alias l.='ls -d .*'
	alias ll='ls -l'
	
	set PATH $HOME/.local/bin $PATH
    set PATH $HOME/.cargo/bin $PATH
 
	function fish_greeting
	end
end
set -x RUSTUP_UPDATE_ROOT https://mirrors.tuna.tsinghua.edu.cn/rustup/rustup
set -x RUSTUP_DIST_SERVER https://mirrors.tuna.tsinghua.edu.cn/rustup
```

## Arduino IDE2 （Flatpak版

Flatpak build of Arduino IDE. To run the app you need USB permissions, preferably, the user has to be part of the `dialout` group. Alternatively, add

```
KERNEL=="ttyUSB[0-9]*",MODE="0666"
KERNEL=="ttyACM[0-9]*",MODE="0666"
```

to `/etc/udev/rules.d/50-arduino.rules`.
