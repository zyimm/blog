---
title: 🫒npm出现error:0308010C:digital envelope routines::unsupported错误解决
date: 2022-10-15
tags: 
    - Npm
    - Node
---

搜索一下可能与node的版本有关，目前可以确认node17以下没啥问题,node18以上需要执行如下命令：

```sh
export NODE_OPTIONS=--openssl-legacy-provider
```

内存溢出的错误如下所示:

> <--- JS stacktrace --->
>>FATAL ERROR: Ineffective mark-compacts near heap limit Allocationfailed - JavaScript heap out of memory
>>
>----- Native stack trace ----

解决办法如下：

```sh
#linux
export NODE_OPTIONS="--max-old-space-size=(X * 1024)" # Increase to X GB
#windows
set NODE_OPTIONS="--max-old-space-size=(X * 1024)" # Increase to X GB
```

参考连接：https://stackoverflow.com/questions/50230339/npm-error-0308010c-digital-envelope-routines-unsupported
