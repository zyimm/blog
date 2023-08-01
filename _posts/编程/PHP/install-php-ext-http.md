---
title: ⛰PHP安装http扩展一些问题解决
date: 2023-08-01
tags: PHP
---
最近在项目中遇到依赖http扩展，出现一些问题，这边小计这些问题解决办法！我的环境是基于docker所以如下命令均在docker容器里面执行！

# 有关错误集锦

以下我遇到错误解决办法

1. checking whether libcurl version  >= 7.18.2... configure: error: no

这个类似这样错误需要确认libcurl,curl-dev是否正确安装，基于不同发行版docker可能包名不一致，需要根据实际情况区别！以下是基于alpine docker 安装命令
```shell
apk  add libcurl curl-dev
```
libcurl curl-dev 是linux下curl开发一系列依赖库和文件。http扩展需要curl，所以需要安装相关依赖！

2. please install and enable pecl/raphf

这个问题需要确认`pecl/raphf`扩展是否安装，如果没有需要执行:
```shell
pecl install pecl/raphf

# 安装完毕之后启用它

docker-php-ext-enable raphf

```
raphf 用于提供高级的哈希函数和消息认证码（MAC），提供一些加密算法，用来保护数据安全性！


# 安装验证

```shell
pecl install pecl_http 
```

看看是否还会报错？