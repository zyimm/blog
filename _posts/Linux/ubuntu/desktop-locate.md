---
title: 💁Ubuntu程序Desktop文件位置
date: 2024-04-06 16:00:00
tags: 
    - Ubuntu
---

Ubuntu的desktop文件相当于windows的桌面软件快捷方式，可以在任何地方快速打开软件。记录一下Ubuntu系统中desktop文件的位置。

方便以后处理删除程序或新增程序时候，桌面图标不正确的问题。

**1 用户的desktop 文件位置**

```sh
~/.local/share/applications
```

**2 公共的 desktop 文件位置,如有重复,用户自己desktop文件优先**

```sh
/usr/share/applications
```

<!--more-->

> `.desktop`桌面文件的一些常见配置项：

1. Name：应用程序的可读名称。
2. Exec：要执行的命令。这通常是启动应用程序的命令。
3. Icon：应用程序的图标文件路径。
4. Type：指定项目的类型，可以是Application、Link等。
5. Categories：指定应用程序所属的类别，比如Utility、Development、Network等。
6. Terminal：指定是否在终端中运行应用程序。
7. StartupNotify：指定是否在应用程序启动时发送通知。
8. MimeType：指定应用程序支持的MIME类型。

**示例参考:**

[启动图标创建示例-https://www.zyimm.com/2022/11/05/Linux/ubuntu/charles-install]
