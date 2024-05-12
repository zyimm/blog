---
title: Ubuntu 22.04.1 修复网卡驱动
date: 2023-01-02
tags: Ubuntu
---

因为笔记的指纹驱动是goodix供应商提供，但是官方没有提供相应的linux下驱动，导致指纹在自己的ubuntu下使用。最近看到[libfprint支持了goodix有关设备](https://fprint.freedesktop.org/supported-devices.html)，其中包含了我的型号(型号查看`lsusb`)，但是安装之后有线网卡没了，且编译安装过程中关于`libusb`无法解决，最终导致安装失败。

所以本次记录修复有线网卡解决办法，有线网卡故障表示`线缆已拔出`，实际是网卡驱动丢失并不是线路故障！

## 解决办法

### 查找网卡设备

```shell
lspci -k |grep Ethernet
```

显示如下:

```shell
01:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8125 2.5GbE Controller (rev 05)
```
<!--more-->
可以看到我的网卡是**Realtek Semiconductor Co., Ltd. RTL8125**,然后到供应商官网寻找对应驱动，[下载地址](https://www.realtek.com/zh/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software)。

### 解压&安装

```shell
#解压
tar vjxf r8125-9.aaa.bb.tar.bz2
#进入目录
cd r8125-9.aaa.bb
#安装&自动处理原先错误驱动
sudo ./autorun.sh
```

### 检测

安装完毕之后，可以通过如下命令验证

```shell
lsmod | grep r8125

ifconfig -a
```

如果上述命令能正常显示有关网卡驱动信息，且系统设置界面按钮可操作则代表成功！
