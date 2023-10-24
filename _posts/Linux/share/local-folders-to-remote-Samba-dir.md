---
title: 🍇本地文件夹映射到远程Samba目录
date: 2023-10-21
tags: Samba
---

家里一台软路由安装了jellyfin，想把媒体目录挂载到另一台大容量的主机上。所以要将这台大容量的主机上通过Samba共享给软路由主机上，因此需将软路由主机本地文件夹映射到远程大容量的主机Samba共享目录下。
<!--more-->
前提Samba安装配置已经在两台主机上配好，这里不再复述！

# 安装软件包

目前可以通过`cifs-utils`工具包实现。cifs-utils 是一个用于在 Linux 系统上实现与 Windows 共享文件夹交互的软件包。它提供了一组工具和库，允许 Linux 系统通过 CIFS（Common Internet File System）协议连接和访问远程 Windows 共享。

> 当然linux与linux之间也是可以的！

```shell
opkg update && opkg install cifs-utils
```

# 挂载远程Samba目录

**1.建立本地文件夹samba-mount**

`mkdir samba-mount`

**2.挂载远程Samba目录**  
假设远程 Samba 服务器的 IP 地址是 `192.168.1.100`，共享目录路径是 `/samba-share`，你的用户名是 `user`，密码是 `password`，可以使用以下命令进行挂载:

```shell
sudo mount -t cifs //192.168.1.100/samba-share samba-mount -o username=user,password=password
```

执行命令后，如果连接成功，远程 Samba 目录将被挂载到本地文件夹 samba-mount。

# 取消挂载

想要取消挂载，可以使用 umount 命令将挂载的目录卸载

```shell
sudo umount samba-mount
```
