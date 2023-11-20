---
title: 🌲Git删除已经提交在远程仓库中的忽略文件
date: 2023-11-18
tags: Git
---

很多时候我们在git库中，不小心把应该忽略的文件或目录提交远程库中，比如`.idea`，`.ivscode`等。把远程删除，本地不发生变化，以下操作即可：

### 1.使用git rm 命令

**git rm -r --cached** 要删除的文件名或目录,如:

```sh
git rm -r --cached .idea #--cached 只删远程仓库的文件,不会删除本地的
```

### 2.提交操作记录描述

```sh
git commit -m '删除XX文件'
```

### 3.推送到远程仓库

```sh
git push -u origin <branch_name>
```
