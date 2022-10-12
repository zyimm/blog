---
title: Ubuntu 22.04.1 snap-store自身更新
date: 2022-10-05
tags: Ubuntu
---

目前开发的笔记本使用的是ubuntu22.04.1 版本，默认内置snap商店无法完成自身更新，所以小记一下解决办法。


解决办法如下：
```sh
# root  账号无需加上sudo

sudo killall snap-store

sudo snap refresh snap-store

```
