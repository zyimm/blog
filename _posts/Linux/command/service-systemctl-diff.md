---
title: 🧺 service 和 systemctl 区别
date: 2023-12-24
tags: 
    - Linux
    - Command
---

比较新的linux服务器，常常用`systemctl` 管理服务，一些比较老的服务器会使用`service`来管理服务，比如一台安装centos 6.5 数据库服务器，那么`service` 和 `systemctl`有哪区别？

## service 命令

`service` 命令是传统的`SysV init` 系统的服务管理工具。它提供了一种简单的方法来启动、停止、重启和查询系统服务的状态。它通过读取位于 /etc/init.d/ 目录下的服务脚本来管理服务。使用 `service` 命令可以使用服务脚本的名称来操作服务，例如：
<!--more-->
1. 启动服务：`service <service_name> start`
2. 停止服务：`service <service_name> stop`
3. 重启服务：`service <service_name> restart`
4. 查询服务：`service <service_name> status`

## systemd 命令

systemctl 命令是新一代的 systemd 系统的服务管理工具。 systemctl 命令，有如下使用方式：

1. 启动服务：`systemctl start <service_name>`
2. 停止服务：`systemctl stop <service_name>`
3. 重启服务：`systemctl restart <service_name>`
4. 查询服务：`systemctl status <service_name>`

除此之外，`systemctl` 还提供了其他有用的功能，如查看日志、查看服务依赖关系、启用/禁用服务自启动等。

### systemd 来源

早年之前Linux 系统使用 `SysV init` 作为初始化系统来启动和管理系统服务。但是`SysV init` 显示出了一些局限性和性能问题，这些问题如下：

1. 启动慢：SysV init 通过顺序启动每个服务来启动系统，这导致启动时间较长。
2. 依赖关系管理困难：在 SysV init 中，管理和解决服务之间的复杂依赖关系需要手动编写和维护启动脚本。
3. 无法并行启动服务：SysV init 无法并行启动服务，因为它们之间可能存在依赖关系。
4. 缺乏动态监控和自愈能力：SysV init 缺乏对服务状态的动态监控和自愈能力，当服务崩溃或失败时，它无法自动恢复。

>> `systemd` 则优化和弥补`SysV init`缺陷

1. 并行启动：systemd 可以并行启动多个服务，提高系统启动速度。
2. 自动处理依赖关系管理：systemd 能够自动解决服务之间的复杂依赖关系，无需手动编写启动脚本。
3. 自动监控和自愈能力：systemd 可以实时监控服务状态，并在服务崩溃或失败时自动重启或修复。
4. 独立单元管理：systemd 使用单元（unit）的概念来管理和配置系统服务，提供更灵活和统一的管理方式。
5. 支持日志记录：systemd 引入了 journald 日志系统，提供更强大和结构化的日志记录功能。

## 区别

说白了`systemd`是`service`替代品，`systemctl` 是`systemd`前端管理工具。对于一些工具，使用原则都是用新不用旧。
