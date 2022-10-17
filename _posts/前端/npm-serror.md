---
title: npm出现error:0308010C:digital envelope routines::unsupported 错误解决
date: 2022-10-15
tags: npm|node
---

搜索一下可能与node的版本有关，目前可以确认node17以下没啥问题,node18以上需要执行如下命令：

```
export NODE_OPTIONS=--openssl-legacy-provider
```
