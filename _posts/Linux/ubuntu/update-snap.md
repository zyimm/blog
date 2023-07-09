---
title: Ubuntu 22.04.1 snap-store自身更新&禁止自动更新
date: 2022-10-05
tags: Ubuntu
---

## snap-store自身更新

目前开发的笔记本使用的是ubuntu22.04.1 版本，默认内置snap商店无法完成自身更新，所以小记一下解决办法。


解决办法如下：
```sh
# root  账号无需加上sudo
sudo killall snap-store

sudo snap refresh snap-store
```


## 禁止自动更新

> 目前snap 稳定版本通道已经支持禁止自动更新，如果下述命令不起作用或报错，请先同上述进行snap-store自身更新

### 全部自动禁止更新
```sh
snap refresh --hold
```

### 禁用某个软件更新，比如火狐firefox
```sh
snap refresh --hold firefox
```

### 通过 --unhold 参数重新启用自动更新

```sh
snap refresh --unhold
# 同理恢复某个软件更新,如火狐
snap refresh --unhold firefox
```