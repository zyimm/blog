---
title: 完全清除docker
date: 2022-10-07
tags: Ubuntu
---

目前开发的笔记本使用的是ubuntu22.04.1 桌面版本，docker-desktop 正好有桌面版本，所以安装桌面版本比较合适。这边小记一下如何清除已有老旧docker，**注意这里是完全清除老旧docker，原来的image和容器都会被清理！**


# 步骤如下：


## 删除某软件及其安装时自动安装的所有包
```sh
sudo apt-get autoremove docker docker-ce docker-engine docker.io containerd runc
```
## 删除无用的相关的配置文件
```sh
dpkg -l | grep docker
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P 
```
<!--more-->
## 卸载没有删除的docker相关插件,比如
```
sudo apt-get autoremove docker-ce-*
```

## 删除docker的相关配置，命令如：
```sh
sudo rm -rf /etc/systemd/system/docker.service.d
sudo rm -rf /var/lib/docker
```

## 最后再查询下docker相关软件包
```
dpkg -l | grep docker
```

## 如果还有包存在，则删除，可能会有多个
```
sudo apt remove --purge xxx# 
# 验证下 此处应该报错
docker -v
```
