---
title: php8 安装 event扩展后报错
date: 2022-10-16 23:10:09
tags: 
    - Docker
---

## 问题

使用php8.1.11-apline镜像构建安装event扩展，构建一点流程没有发生任何错误，但是启动容器之后报如下错误：

```php
Warning: PHP Startup: Unable to load dynamic library 'event' (tried: /usr/local/lib/php/extensions/no-debug-non-zts-20210902/event (/usr/local/lib/php/extensions/no-debug
-non-zts-20210902/event: cannot open shared object file: No such file or directory), /usr/local/lib/php/extensions/no-debug-non-zts-20210902/event.so (/usr/local/lib/php/
extensions/no-debug-non-zts-20210902/event.so: undefined symbol: socket_ce)) in Unknown on line 0

```

后来搜索一下得知是因为event与另外的sockets扩展启动顺序问题，sockets需在event之前启动！

## 解决办法

将`/usr/local/etc/php/conf.d/`目录下的文件**docker-php-ext-event.ini**重命名为**docker-php-ext-z-event.ini**，让它排在 docker-php-ext-sockets.ini后面即可。

## 参考链接

[php8 安装 event 扩展后报错](https://www.yangdx.com/2022/08/219.html)
