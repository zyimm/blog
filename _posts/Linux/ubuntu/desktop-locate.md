---
title: 🫶Ubuntu24.04笔记本合盖之后无法唤起
date: 2024-05-16
tags: 
    - Ubuntu
---

Ubuntu24.04笔记本合盖之后，无法唤起的问题解决方法如下：

使用root权限修改`/etc/systemd/logind.conf` 下面几个选项配置，如果前面有`#`的话，请先去除注释符号。

```sh
HandleLidSwitch=suspend # 当笔记本电脑使用电池供电时，合盖挂起
HandleLidSwitchExternalPower=suspend #当笔记本电脑插入电源插座时，合盖挂起
HandleLidSwitchDocked=ignore #当笔记本电脑连接到扩展坞时，合盖忽略

# 修改如下：
HandleLidSwitch=ignore 
HandleLidSwitchExternalPower=ignore 
HandleLidSwitchDocked=ignore

```

**保存之后重启登陆即可！**

> 参数的可选值解释如下：

1. suspend：合盖时挂起
2. lock：合盖时锁定
3. ignore：什么都不做
4. poweroff：关机
5. hibernate：合盖时休眠

设置成ignore避免笔记盒盖导致挂起或休眠，之后的无法唤起故障！
