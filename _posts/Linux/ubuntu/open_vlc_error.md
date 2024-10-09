---
title: 🧌 ubuntu无法打开vlc,Qt: Session management error: Could not open network socket 错误
date: 2024-10-07
tags: 
    - Ubuntu
---

Ubuntu 安装vlc之后无法正常打开，命令行输入`vlc`，显示如下错误信息：

```sh
> vlc
/usr/share/libdrm/amdgpu.ids: No such file or directory
amdgpu: unknown (family_id, chip_external_rev): (146, 32)
libGL error: failed to create dri screen
libGL error: failed to load driver: radeonsi
VLC media player 3.0.20 Vetinari (revision 3.0.20-1-g2617de71b6)
[00006310db453a10] main libvlc: 正在以默认界面运行 vlc。使用“cvlc”可以无界面模式使用 vlc。
Warning: Ignoring XDG_SESSION_TYPE=wayland on Gnome. Use QT_QPA_PLATFORM=wayland to run on Wayland anyway.
Qt: Session management error: Could not open network socket
Fontconfig warning: FcPattern object width does not accept value [75 100)
[1]    238348 segmentation fault (core dumped)  vlc
```
<!--more-->

解决方案如下：

```sh
sudo rm /var/cache/fontconfig/*
rm ~/.cache/fontconfig/*
fc-cache -r
```

上述命令，执行完之后，再次打开vlc，需要等待一段时间，然后就可以正常使用。

第二种解决方案是使用 `sudo apt install vlc` 安装，不要用snap。先卸载snap的vlc，再安装，就可以正常打开了。

具体是什么原因不是很清楚，本地网络不通也会出现这个错误。
