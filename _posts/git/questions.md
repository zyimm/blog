---
title: 💯git一些常见问题解决
date: 2023-10-06
tags: Git
---

本篇文章记录git一些常见问题解决，不定时更新！

### 1.You asked to pull from the remote 'gitea', but did not specify a branch. Because this is not the default configured remote for your current branch, you must specify a branch on the command line

这个错误提示说明在从远程仓库拉取代码时没有指定分支。由于当前分支不是默认配置的远程分支，所以需要在命令行中指定分支。

要解决这个问题，可以使用以下命令来拉取指定分支的代码：

```sh
git pull <remote> <branch>
```

### 2.error: RPC 失败。HTTP 413 curl 22 The requested URL returned error: 413 send-pack: unexpected disconnect while reading sideband packet

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

### 3.fatal: 拒绝合并无关的历史

这个错误通常发生在尝试合并两个没有共同历史的分支时。解决这个问题的一种方法是使用--allow-unrelated-histories选项来强制合并。命令如下：

```sh
git merge --allow-unrelated-histories <branch_name>
```

这个命令将允许你合并两个没有共同历史的分支。如果有冲突发生，需要解决冲突并手动提交合并结果。

### 4.查看某个文件提交记录

要查看文件在Git中的提交记录，可以使用以下命令：

```sh
git log -- <file_path>
```

其中<file_path>是您想要查看提交记录的文件路径。这个命令将显示指定文件的提交历史，包括提交者、提交信息、提交时间等信息。

如果您只想查看最近的提交记录，您可以使用-n选项来指定要显示的提交记录数量，例如：

```sh
git log -n 5 -- <file_path>
```

这将显示最近的5次提交记录。

另外，如果您想查看某个特定文件在每次提交中的变更，您可以使用以下命令：

```sh
git log -p -- <file_path>
```

这个命令将显示每次提交对指定文件所做的具体变更。

### 5.从远程checkout分支并关联

```sh
git  checkout -b <branch_name> <remote>/<branch_name>
```

or

```sh
git  checkout --track <remote>/<branch_name>
```
