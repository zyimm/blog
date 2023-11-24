---
title: 🆒Git 批量删除本地以及远程分支
date: 2023-03-29
tags: Git
---

### 1. 查看所有本地分支

```sh
git branch
```

### 2. 删除本地分支

```sh
git branch -D branch_name
```

其中，branch_name为要删除的分支名称。

### 3. 查看所有远程分支

```sh
git branch -r
```

### 4. 删除远程分支

```shell
git push origin --delete branch_name
```
<!--more-->
> 如果要批量删除本地分支，可以使用以下命令：

```shell
git branch | grep -v "main" | xargs git branch -D
```

其中，"main"为要保留的分支名称，可以根据需要进行修改。

如果要批量删除远程分支，可以使用以下命令：

```shell
git branch -r | grep -v "main" | sed 's/origin\//:/' |  xargs git push
```

`main`为要保留的分支名称，可以根据需要进行修改。最后一个命令中的sed命令是将远程分支名中的`origin/`替换为`:/`，因为git push命令中需要使用的远程分支名不包含`origin/`

### 5. 删除远程分支已经删除，但本地仍然存在的分支

```shell
git remote prune origin
```

该命令会检查本地仓库中的远程分支引用，并将那些在远程仓库中已经不存在的分支引用删除掉，以保持本地仓库的分支列表与远程仓库保持同步。
