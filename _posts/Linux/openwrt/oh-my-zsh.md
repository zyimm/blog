---
title: 👨‍🍼openwrt24.10安装oh-my-zsh
date: 2025-04-01
tags: 
    - openwrt
    - oh-my-zsh
---

openwrt24.10安装oh-my-zsh遇到一些问题记录如下：

## 1. 更新软件包列表

```sh
opkg update
```

## 2. 安装Zsh

```sh
opkg install zsh

# 如果没有curl或wget则需要安装其中一个
opkg install wget
# 或者
opkg install curl

# 下载oh-my-zsh
wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O - | zsh
# 或者
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

#可能出现的错误“git: 'remote-https' is not a git command. See 'git --help'.” 需要移除默认git使用git-http替代

opkg remove git
opkg install git-http

```

## 3. 配置Zsh

后续补充！
