---
title: docker exec 执行命令
date: 2023-02-11
tags: Docker
---

目前一些场景在运行的容器时候需要附带执行一些命令，可以使用`docker exec`命令

`docker exec [OPTIONS] CONTAINER COMMAND [ARG...]`


OPTIONS说明：

    -d,–detach :分离模式: 在后台运行

    -i,–interactive :即使没有附加也保持STDIN 打开

    -t,--tty:分配一个伪终端

    -u,-user :指定执行用户

    -w,–workdir :指定工作目录


## docker 定时任务执行
> 下面是PHP的satis私有镜像仓库命令在docker中同步命令如下：
<!--more-->
```shell
docker exec php74 /bin/sh -c "cd satis/ && php bin/satis build satis.json public/"
```
1. `php74` 指定容器名称
2. `/bin/sh` 指定shell环境
3. `-c "cd satis/ && php bin/satis build satis.json public/"`  具体命令执行详情


> 且让它每分钟执行一次，执行`crontab -e` 新增如下代码:

```shell
* * * * * docker exec php74 /bin/sh -c "cd satis/ && php bin/satis build satis.json public/" > /dev/null 2>&1
```

保存即可！

### 注意
1. 定时任务不需要开启一个终端，所以命令 `docker exec` 后面无需跟`-t` 分配一个终端
2. 每个容器交互shell有bash或sh，命令中`/bin/sh` 应是具体容器交互shell决定使用哪个

## 参考
1. [linux crontab命令 参考](http://linux.zyimm.com/c/crontab.html)
2. [crontab 使用 ](https://learnku.com/articles/26172)