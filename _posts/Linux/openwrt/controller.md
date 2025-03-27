---
title: 💯openwrt24.10上网控制
date: 2025-03-27
tags: 
    - openwrt
---

openwrt24.10上网控制，可以使用[luci-app-timecontrol](https://github.com/sirpdboy/luci-app-timecontrol)

该插件适配24.10分支的NFT的上网时间控制插件。 **编译安装或直接ipk安装二选一**。

## 编译安装

```sh
# feeds获取源码：
src-git timecontrol  https://github.com/sirpdboy/luci-app-timecontrol

scripts/feeds update timecontrol
scripts/feeds install luci-app-timecontrol

# 配置菜单
make menuconfig
# 找到 LuCI -> Applications, 选择nft-timecontrol, 保存后退出。

# 编译固件
 make package/luci-app-timecontrol/compile V=s
```

## 直接ipk安装

下载[luci-app-timecontrol](https://github.com/sirpdboy/luci-app-timecontrol/releases)比较新的ipk包，直接安装即可，无需编译。

```sh
opkg update

opkg install <下载的ipk包>
```
