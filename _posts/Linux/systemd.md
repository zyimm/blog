---
title: 🔰systemd服务单元文件讲解
date: 2024-03-16
tags: Linux
---

systemd服务单元文件主要放在`/usr/lib/systemd/system`或`/etc/systemd/system`目录下，文件一般以 `.service` 为后缀。找到配置文件以后，使用文本编辑器打开即可。

也可以`systemctl cat`命令可以用来查看配置文件。

## systemd服务单元文件

`systemd`服务单元文件是服务系统自启动的一种脚本。以下是一个示例：

```sh
[Unit]
Description=My Sample Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/php /path/to/your/script.php
Restart=always

[Install]
WantedBy=multi-user.target
```
<!--more-->
将这个服务单元文件保存在 /etc/systemd/system/ 目录下。使用以下命令启用这个服务并启动它：

```sh
sudo systemctl enable my-service.service # 启用,随机启动
sudo systemctl start my-service.service # 启动
```

## 常见配置讲解

### Unit 配置

1. Description：描述服务的简要说明。
2. Documentation：服务的文档URL。
3. Requires：指定服务所依赖的其他服务。
4. After：指定服务的启动顺序,表示在服务之后启动。
5. Before：指定服务的启动顺序,表示在服务之前启动。
6. Condition：指定服务的启动条件。

### Service 配置

1. ExecStart：指定启动服务时执行的命令。
2. ExecStop：指定停止服务时执行的命令。
3. Restart：指定服务异常退出时的重启策略。
   - always：表示无论何时服务退出（无论是正常退出还是异常退出），Systemd都会自动尝试重新启动该服务。
   - on-success：仅在服务正常退出时重启。
   - on-failure：仅在服务异常退出时重启。
   - on-abnormal：在服务异常退出时重启。
   - on-abort：在服务收到中止信号时重启。
4. User：指定服务的用户。
5. WorkingDirectory：指定服务的工作目录，注意需要使用绝对路径。
6. Environment：指定服务的环境变量。

### Install 配置段

WantedBy：用于指定哪些Systemd目标服务会依赖当前服务。Systemd目标服务是一种特殊的服务单元，用于控制系统启动和关闭的过程。可选如下：

1. multi-user.target：多用户目标，表示系统处于正常运行状态。
2. graphical.target：图形界面目标，表示系统启动到图形界面。
3. rescue.target：紧急恢复目标，用于系统无法启动时进入紧急模式。
4. halt.target：关机目标，表示关闭系统。
5. reboot.target：重启目标，表示重启系统。

## systemctl命令使用

systemctl是Systemd前端管理工具，用于管理Systemd服务和系统状态。

1. systemctl list-dependencies 命令列出一个 Unit 的所有依赖。
2. sysystemctl status <service-name> 命令用于查看系统状态和单个 Unit 的状态。
3. systemctl list-units命令可以查看当前系统的所有 Unit。
4. systemctl list-unit-files命令可以查看当前系统的所有 Unit 文件。
5. systemctl restart <service-name> 命令用于重启单个 Unit。
6. systemctl start <service-name> 命令用于启动单个 Unit。
7. systemctl stop <service-name> 命令用于停止单个 Unit。
8. systemctl enable <service-name> 命令用于开启单个 Unit 的自动启动。
9. systemctl disable <service-name> 命令用于禁用单个 Unit 的自动启动。

## 参考

[阮一峰：Systemd 入门教程](https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)
