---
title: Git 免密登录
date: 2023-04-01
tags: Git
---

## [方式一] 配置ssh实现免密登录

> 本地需要安装OpenSS扩展

### 第一步 通过终端生成rsa key

```sh
ssh-keygen -t rsa -C "your_email@example.com"  
```

`your_email@example.com` 是你自己的邮箱域名，该命令一路回车，生成一对rsa密钥并将电子邮件地址设置为`your_email@example.com`，你git仓库使用的邮箱与此对应。

### 第二步 git中设置密钥

```sh
git config --global core.sshCommand "openssh-client -o StrictHostKeyChecking=no -i /path/to/your/key.pem"  
```

1. `--global` 参数代表全局使用，不全局使用则需要进入对应本地仓库目录进行设置

2. `/path/to/your/key.pem` 是你的私钥

**或将SSH密钥添加到ssh-agent**

- 在后台启动 ssh 代理

```sh
eval "$(ssh-agent -s)"
```

- 添加私钥

```sh
 ssh-add ~/.ssh/私钥
```

### 第三步 远程仓库设置公钥

远程仓库如github,gitlab 会有一个设置保存ssh地方，将你的公钥也就是.pub结尾的复制上去便可！

> git的ssh和linux ssh免密登录是原理一致的！

## [方式二] 使用credential.helper

`credential.helper` 参数配置用于指定 Git 在连接远程服务器时使用的密码存储程序。默认情况下，Git 使用 ~/.ssh/id_rsa.pub 文件存储密码，但可以使用 store 选项来更改该默认设置。

`credential.helper=store` 将使用密码存储程序来存储远程服务器的用户名和密码，以便在以后使用。命令如下:

```ssh
git config  credential.helper 'store'    
```

接下来默认只需要第一次登录使用密码之后，就会记住密码，下次无需在使用密码登录！

### 清除密码

```sh
git config  credential.helper null
```
