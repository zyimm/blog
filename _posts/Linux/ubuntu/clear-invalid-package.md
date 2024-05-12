---
title: Ubuntu 22.04.1 清理安装失败或配置错误的软件包
date: 2022-12-31
tags: Ubuntu
---

目前开发的笔记本使用的是ubuntu22.04.1 版本，时间长了系统里面或出现一些安装失败或配置错误的软件包，记录一下处理该问题的方法。

## 解决办法

> 命令如下：
```shell
# root  账号无需加上sudo
sudo apt purge $(dpkg -l|grep ^rc|awk '{ print $2 }')
sudo apt purge $(dpkg -l|grep iF|awk '{ print $2 }')
```
<!--more-->
> 注解：
1. `$(......)` 是一个shell表示法，即里面包含括号中的命令输出的内容。
2. `dpkg -l`列出系统中所有安装的软件，如果是已经删除的软件（有残存的配置文件），那么该的软件包的状态是rc，即开头显赫为rc 然后是空格，然后是软件包的名称。如果是iF开头就是配置失败的软件。
3. `grep ^rc` 的用处就是找出状态为rc的所有软件包，即以rc开头的行。`|awk '{ print $2 }'` awk可以将输入的字符串用指定的分隔符进行分解，缺省情况下是空格，$2是表示第二个字段，也就是软件包的名称，因为第一个字段是 rc。
4. `apt-get purge` 就是彻底删除软件包（包括配置文件），如果是残存的配置文件，也可以用这种方式删除。

## 参考
1. [Ubuntu清理配置错误或安装失败的包](https://neilwan.com/views/linux/ubuntu_clean_install_failed_package.html)
