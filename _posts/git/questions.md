---
title: 💯git一些常见问题解决
date: 2023-10-06
tags: Git
---

本篇文章记录git一些常见问题解决，不定时更新！

1.**You asked to pull from the remote 'gitea', but did not specify a branch. Because this is not the default configured remote for your current branch, you must specify a branch on the command line**

这个错误提示说明在从远程仓库拉取代码时没有指定分支。由于当前分支不是默认配置的远程分支，所以需要在命令行中指定分支。

要解决这个问题，可以使用以下命令来拉取指定分支的代码：

```sh
git pull <remote> <branch>
```

2.**error: RPC 失败。HTTP 413 curl 22 The requested URL returned error: 413 send-pack: unexpected disconnect while reading sideband packet**

这个错误提示说明推送包（push package）太大了，超过了服务器所允许的大小限制。

要解决这个问题，有如下操作：

1.增加 Git 中缓存的默认限制：

```bash
git config --global http.postBuffer 10240000000
```

2.服务端是用nginx反向代理的，修改nginx配置:

```sh
server {
    // 其他配置省略
    client_max_body_size 500m;
}
```
