---
title: Linux Command 本地镜像
---

方便工作学习中查找linux方便的命令，特地基于Linux Command建立这个镜像站点！

地址：[Linux Command建立这个镜像站点！](http://linux.zyimm.com/)

## docker 安装
```sh
docker pull ghcr.io/jaywcjlove/linux-command:latest

# 自定义端口
docker run --name linux-command --rm -d -p 3100:3000 wcjiang/linux-command:latest
```

在浏览器中访问以下 URL

`http://localhost:3100/`