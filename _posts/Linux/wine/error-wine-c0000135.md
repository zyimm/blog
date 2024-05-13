---
title: 🫥wine运行c0000135错误解决
date: 2024-05-12
tags: 
    - Ubuntu
    - Wine
---

安装wine

```sh
sudo dpkg --add-architecture i386 && sudo apt install wine
```

运行wine时出现"could not load kernel32.dll, status c0000135"错误，解决办法如下：

1. 删除目录`~/.wine`目录
2. 运行`winecfg 让系统重新生产被删除的配置文件即可
