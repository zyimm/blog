---
title: 👨‍🍼wsl下访问移动U盘硬盘
date: 2024-12-14
tags: 
    - Wsl
    - Linux
---

wsl下在/mnt/目录下，访问windows硬盘分区比如常见的C盘等，但移动U盘或硬盘，却不在/mnt/目录下，需要手动挂载。

在windows下，看一下移动U盘或硬盘分配的盘符，比如是“F”，然后将其挂载到/mnt/目录下即可。具体操作如下：

```sh
# 挂载硬盘

sudo mkdir /mnt/f

sudo mount -t drvfs F: /mnt/f

```

注意：

1. -t drvfs：-t 参数指定了文件系统的类型，这里使用的是 drvfs。drvfs 是用于WSL（Windows Subsystem for Linux）环境中的文件系统类型，它允许将Windows驱动器挂载到WSL环境中,而不是ntfs。
2. 上述操作是在wsl下。
