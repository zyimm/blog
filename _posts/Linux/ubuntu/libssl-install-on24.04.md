---
title: 🐕Ubuntu 24.04 上安装libssl1.1
date: 2024-05-11
tags: 
    - Ubuntu
---

在ubuntu 24.04上安装linux原生微信客户端时出现的问题：

下列软件包有未满足的依赖关系：
 com.tencent.wechat : 依赖: libssl1.1 (>= 1.1.0) 但无法安装它
E: 无法修正错误，因为您要求某些软件包保持现状，就是它们破坏了软件包间的依赖关系。

**解决办法：**
安装libssl1.1

```sh
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb
sudo apt  install libssl1.1_1.1.0g-2ubuntu4_amd64.deb
```
