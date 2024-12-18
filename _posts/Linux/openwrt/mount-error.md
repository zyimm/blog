---
title: 💯openwrt Errorr elocating /usr/bin/mount:mnt_fs_is_regularfs:symbol not found错误
date: 2024-12-18
tags: 
    - openwrt
---

openwrt将目录挂载到新硬盘出现“Errorr elocating /usr/bin/mount:mnt_fs_is_regularfs:symbol not found”，记录一下解决办法。

### 出现错误可能的原因

mount命令尝试使用的库函数mnt_fs_is_regularfs在运行时，mount依赖fdisk(这个是具体环境)libblkid libfdisk libmount libuuid 等包，没有安装完整导致的。

### 解决办法

```sh
opkg update
opkg install --force-reinstall fdisk libblkid libfdisk libmount libuuid
```

使用 --force-reinstall 选项强制重新安装包，确保所有依赖项都正确安装。
