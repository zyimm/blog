---
title: Linux Command 本地镜像
date: 202-10-10
tags: Linux
---

方便工作学习中查找linux的命令(因为自己搞容易记得 😂 )，特地基于Linux Command建立这个镜像站点！

地址❤️：[Linux Command建立这个镜像站点！http://linux.zyimm.com](http://linux.zyimm.com/)

## docker 安装

```shell
# 拉取镜像
docker pull ghcr.io/jaywcjlove/linux-command:latest
# 自定义端口
docker run --name linux-command --rm -d -p 3100:3000 wcjiang/linux-command:latest
```
<!--more-->
在浏览器中访问如下地址:

`http://localhost:3100/`

## 参考

- [linux-command https://github.com/jaywcjlove/linux-command](https://github.com/jaywcjlove/linux-command)
- [Markdown官方教程 https://markdown.com.cn](https://markdown.com.cn/intro.html)
- [emoji表情参考 https://emojipedia.org](https://emojipedia.org/)

## 推荐

其实我还推荐一个关于linux命令参考网站：[www.linuxcool.com(命令示例也不错！)](https://www.linuxcool.com/)
