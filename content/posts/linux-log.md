---
title: "Linux折腾大杂烩"
date: "2022-04-13"
tags: ["瞎折腾","Linux"]
---
# gnome改动

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

# grub修改

修改grub.cfg的权限`sudo chmod +w /boot/grub/grub.cfg`
输入`sudo gedit /boot/grub/grub.cfg`对文件进行编辑后保存
输入`sudo chmod -w /boot/grub/grub.cfg`将文件权限改回只读
