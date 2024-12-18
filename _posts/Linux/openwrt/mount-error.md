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
<!--more-->

### 自动挂载

如果不需要自动开机挂载，这一步可以跳过。

编辑/etc/config/fstab文件中，可以自动挂载硬盘。前提需要已经安装了block-mount和kmod-fs-<filesystem>（例如ext4, ntfs等）以及任何其他可能需要的内核模块，安装命令如下：

```sh
opkg update
opkg install block-mount kmod-fs-ext4 #这里是ext4类型
```

然后编辑/etc/config/fstab文件，添加如下条目：

```sh
config mount
    option target '/mnt/mydrive'    # 挂载点目录 如果没有请先创建
    option device '/dev/sda1'       # 硬盘分区设备名 (可以是UUID=...)
    option fstype 'ext4'            # 文件系统类型
    option options 'rw,sync'         # 挂载选项
    option enabled '1'              # 是否启用此条目 (0为禁用)
    option enabled_fsck '0'         # 是否在启动时检查文件系统 (0为不检查)
```
