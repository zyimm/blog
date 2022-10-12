---
title: Centos下源码编译git
date: 2022-09-30
tags: Git
---

最近向公司申请一台闲置的centos主机，用来安装composer 私有镜像仓库使用的，不得不说目前任职公司PHP项目部署还是人工手动ftp上传code年代，composer依赖包也没有用起来。所以目前第一步先建立一个composer镜像仓库，把composer包管理用起来,安装之前先把git安装起来。

centos7.9 默认git版本是1.8，版本比较低。目前一些场景需要用到git2.0版本之上，需要源码编译安装最新的版本。

## 下载源码

```
git clone https://github.com/git/git
// 或直接下包
wget https://codeload.github.com/git/git/tar.gz/refs/tags/v2.37.3

```

## 进入目录&编译

### 卸载旧git&解压文件

```sh
# 卸载旧git
sudo yum remove  git
# 解压
tar xvf git-v2.37.3.tar.gz
# 进入目录
cd git-2.37.3
```

> 编译时候出现一些依赖问题，所以建议预先检查这些依赖是否安装,从而避免编译失败

- [ ] gcc
- [ ] curl-devel
- [ ] openssl-devel
- [ ] zlib-devel
- [ ] expat-devel
- [ ] gettext-devel

### 安装依赖

```
sudo yum install -y gcc curl-devel openssl-devel zlib-devel expat-devel gettext-devel
```

### 编译

```
# $(nproc) 可以根据实际需求自行替换，减少编译时间
sudo make -j$(nproc) && make install
```


## 验证

```
git  --version
// git version 2.37.3 输出如下命令就代表编译安装成功！
```