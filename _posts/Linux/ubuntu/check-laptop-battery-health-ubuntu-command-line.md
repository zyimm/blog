---
title: 👁️Ubuntu下检查电池健康度
date: 2024-11-01
tags: 
    - Ubuntu
---

我的笔记本电脑使用Ubuntu以来，一直存在电池反复充电的问题，在充电到100%之后仍然不停微充电，来回在99-100%之间循环！

一般来讲,正常的电池优化管理应该是低于某个设定数值比如98%就停止充电，以便提高电池寿命！

所以我找一个好用的命令行工具来检查电池健康度。使用教程如下：

1. 打开终端，输入命令`upower --enumerate`
2. 复制打印的电池路径（通常以 结尾_BAT0），比如我的电池路径是`/org/freedesktop/UPower/devices/battery_BATT`
3. 输入upower -i 并粘贴2中的电池设备路径
<!--more-->
执行结果如下图:

![[alt text](image.png)](https://www.zyimm.com/images/media/20241101/a9f8daad150af13472ac49174a1601e0.png)

> [!TIP]一些重要参数解释：

- `energy-full`：电池现在可冲进去的最大容量
- `energy-full-design`：电池的出厂设计最大的容量
- `charge-cycles`: 充电循环次数，一般笔记本电池寿命在300～500次

## 参考

[check-laptop-battery-health-ubuntu-command-line](https://www.omgubuntu.co.uk/2024/08/check-laptop-battery-health-ubuntu-command-line)