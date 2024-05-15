---
title: 🧚Ubuntu安装程序出现deepin-elf-verify依赖错误
date: 2024-05-15
tags: 
    - Ubuntu
    - Deepin
---

在ubuntu 上安装一些从uos或deepin上面的软件时，会出现 deepin-elf-verify依赖错误。

deepin-elf-verify究竟是何物？

总结：deepin-elf-verify是信任机制，只有在信任的才能安装软件，相当于官方简单认证。

详细可以参考这篇文章：[deepin-elf-verify是什么](https://zhul.in/2021/11/20/what-is-deepin-elf-verify/)

ubuntu的仓库是没有这个软件的所以无法安装。

<!--more-->

解决如下：

1. 解包&修改文件

```sh
sudo  dpkg-deb -R  [你的.deb包路径] [解包目录] #如sudo  dpkg-deb -R  xx.deb tmp

#解包如下：

├── DEBIAN
│   └── control
└── opt
    └── apps

#修改DEBIAN/control文件如下:

将Depends：后面包含deepin-elf-verify 删除掉即可
```

2. 重新打包

```sh
sudo  dpkg-deb -b   [解包目录] [新包名] #如sudo  dpkg-deb -b  tmp xx.deb
```

重新安装新的打包即可！
