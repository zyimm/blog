---
title: 🧑‍🚒ubuntu上编译openwrt的若干问题记录
date: 2024-07-06
tags: 
    - openwrt
---

在ubuntu 24.04上编译openwrt时，出现一些问题，记录一下。

## 1.OpenWrt根目录镜像小

编译完的img镜像只有160M，不方便后期安装更多软件，解决如下：
 <!--more-->
1. 在编译的时候设置根目录空间大小
2. 首先选择Target Images选项
![da48017961c1e756b12302941530c3cb](https://www.zyimm.com/images/media/20240906/da48017961c1e756b12302941530c3cb.jpg)

- 先勾选上ext4选项
- 然后`在Root filesystem partition size (in MB)`选项，调整根目录空间大小（MB）

![a9340335271eb70e31c5c5215e62d68a](https://www.zyimm.com/images/media/20240906/a9340335271eb70e31c5c5215e62d68a.jpg)

3. 关于ext4与squashfs两种固件镜像文件系统格式：
    3.1 squashfs格式的固件，支持在面板内恢复初始状态；ext4格式的固件则不可以；
    3.2 ext4格式的固件可以灵活调整分区大小；squashfs格式的固件则不可以；

## 2.libncurses缺失 解决如下

在新的ubuntu 24.04仓库中目前已经没有libncurses这个软件包了，所以需要手动下载安装包

```sh
 wget http://archive.ubuntu.com/ubuntu/pool/universe/n/ncurses/libtinfo5_6.4-2_amd64.deb && sudo dpkg -i libtinfo5_6.4-2_amd64.deb && rm -f libtinfo5_6.4-2_amd64.deb

wget http://archive.ubuntu.com/ubuntu/pool/universe/n/ncurses/libncurses5_6.4-2_amd64.deb && sudo dpkg -i libncurses5_6.4-2_amd64.deb && rm -f libncurses5_6.4-2_amd64.deb

sudo apt install lib32ncurses5-dev libncurses5 libncurses5-dev -y 
```

## 3.make defconfig 有什么作用?

`make defconfig` 用于只提供 `.config` 部分代码片段的默认值补全，也就是用`.config`代码片段  **defconfig** 出完整的`.config`用于编译。简单理解就是不存在的配置项自动补全默认，存在则不更新。常常如下组合使用：

```sh
rm -f ./.config
touch ./.config
cat >> .config <<EOF
#设备型号
CONFIG_TARGET_ramips=y
CONFIG_TARGET_ramips_mt7621=y
........
EOF

make defconfig
make download -j$(nproc)
make -j$(nproc) V=s
```

## 4.docker 编译

进入“Utilities” 选择你需要的容器工具，比如“docker”。

<https://dachunlv.com/2022/03/12/linux/operation/OpenWrt%E7%BC%96%E8%AF%91%E8%BF%87%E7%A8%8B%E8%AE%B0%E5%BD%95/>

## 5.安装dockerman

1. 安装luci-app-ttyd 依赖

```sh
opkg  update
opkg install luci-app-ttyd
```

2. 安装对应ipk

<https://github.com/lisaac/luci-app-dockerman/releases>
