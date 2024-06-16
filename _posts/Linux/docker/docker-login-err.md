---
title: 🔐docker login 阿里云失败问题的解决
date: 2022-10-19 23:10:09
tags: 
    - Docker
---

## 问题

使用阿里云镜像服务进行 docker login 发生错误，具体如下：

```shell
docker login失败：err: exit status 1, Try “pass init“
```

后来搜索一下，找到问题解决办法！

<!--more-->

## 解决办法

### 1.安装 docker-credential-pass

```shell
# 安装docker-credential-pass 最新版本是0.8.2  详细请参考 https://github.com/docker/docker-credential-helpers/releases
# 本次使用的是0.6.0 可以替换成最新版本使用
wget https://github.com/docker/docker-credential-helpers/releases/download/v0.6.0/docker-credential-pass-v0.6.0-amd64.tar.gz

tar -xf docker-credential-pass-v0.6.0-amd64.tar.gz

mv docker-credential-pass /usr/bin/.

docker-credential-pass //出现 You should see: "Usage: docker-credential-pass <store|get|erase|list|version>".

```

### 2.安装 gpg pass & 生成key

```shell
apt install gpg pass # gpg是一个开源的加密软件，pass用于存储和管理密码。它使用GPG进行加密，以确保密码的安全性

# 下一步 生成key
gpg --generate-key #需要要填入姓名邮箱等信息. 输入新的password之后会产生一个新的key

# 复制上一步生成key
pass init (paste from clipboard) //直接从复制版上粘贴

```

### 3.设置密码

```shell
pass insert docker-credential-helpers/docker-pass-initialized-check # 输入新密码
```

### 4. 修改~/.docker/config.json 没有就创建

> credsStore 属性用于指定后端的凭证存储

```json
{
    "credsStore":"pass" 
}
```

## 参考链接

[https://github.com/docker/docker-credential-helpers/issues/102](https://github.com/docker/docker-credential-helpers/issues/102)
