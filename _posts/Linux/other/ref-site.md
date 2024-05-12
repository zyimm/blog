---
title: 🏕️开发工具-速查表
date: 2024-04-23
tags: Linux
---

是为了方便自己的技术栈查阅，以及较遗忘知识点快速找到，所以我也建立了这个站点。

地址❤️：[开发工具-速查表！http://ref.zyimm.com](http://ref.zyimm.com/)

如果需要自己部署，可以参考如下docker部署:

## docker 安装

```shell
# 拉取镜像
docker pull wcjiang/reference
# 自定义端口
docker run --name reference --rm -d -p 9667:3000 wcjiang/reference:latest
# Or
docker run --name reference -itd -p 9667:3000 wcjiang/reference:latest
```
<!--more-->
在浏览器中访问如下地址:

`http://localhost:9667/`