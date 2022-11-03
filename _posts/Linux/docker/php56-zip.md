---
title: php56 安装zip 扩展报错
date: 2022-10-31 23:10:09
tags: Docker
---

## 问题

因为部分老项目使用5.6版本且部分功能依赖zip扩展 ，所以在镜像(apline)构建安装zip扩展，安装过程中报如下错误：

```
Please reinstall the libzip distribution

```



## 解决办法

```sh
apk update 

apk  add zlib-dev

pecl install zip // 或 docker-php-ext-install zip

docker-php-ext-enable zip
```

## 补充
可能还有其他问题，不过我本机可能只缺少这些依赖,所以安装成功了
