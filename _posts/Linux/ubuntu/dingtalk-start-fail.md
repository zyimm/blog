---
title: 🧌 Ubuntu安装钉钉无法正常启动解决
date: 2026-03-12
tags: 
    - Ubuntu
    - 钉钉
---

 Ubuntu安装钉钉之后，钉钉启动报如下错误：

 ```shell
preload_libs=./libgbm.so ./plugins/dtwebview/libcef.so
ERROR: ld.so: object './libgbm.so' from LD_PRELOAD cannot be preloaded (cannot open shared object file): ignored.
Run Main is_gpu=0 is_zygote=0 is_render=0 is_crashpad_handler=0 cmd : ./com.alibabainc.dingtalk %u 
Load /opt/apps/com.alibabainc.dingtalk/files/8.1.0-Release.6021101//dingtalk_dll.so failed! Err=/opt/apps/com.alibabainc.dingtalk/files/8.1.0-Release.6021101//dingtalk_dll.so: cannot enable executable stack as shared object requires: Invalid argument
 ```

> 主要解决两个问题一个是libgbm.so 不存在，二是dingtalk_dll.so 无法启用可执行栈

## 1.libgbm.so 不存在

libgbm.so 是 Mesa 图形库的一部分，正常ubuntu默认已经安装，如果没有安装请执行：

```shell
sudo apt install libgbm1
```
<!--more-->

```shell
#使用 ldconfig 查找 libgbm.so 所在位置

ldconfig -p | grep libgbm

#上述命令输出如下：

libgbm.so.1 (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libgbm.so.1

libgbm.so (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libgbm.so

```

/usr/lib/x86_64-linux-gnu/libgbm.so 文件路径修改钉钉启动文件上(/opt/apps/com.alibabainc.dingtalk/files/Elevator.sh)中`preload_libs=`路径修改成`/usr/lib/x86_64-linux-gnu/libgbm.so`



## 2.dingtalk_dll.so 无法启用可执行栈

导致该错误可能的原因：

1. 某些旧版或非标准编译的 .so 文件（如钉钉的 dingtalk_dll.so）可能标记了需要 可执行栈（executable stack）。
2. 但现代 Linux 系统出于安全考虑（NX/DEP 保护），默认禁止共享库使用可执行栈。
3. 如果内核或安全策略（如 SELinux、PaX、grsecurity 或某些国产 OS 的加固策略）严格限制，就会报 Invalid argument。

ubuntu 25.04 之前版本可以使用execstack工具清除可执行栈标记,命令如下：

```sh
sudo execstack -c /opt/apps/com.alibabainc.dingtalk/files/8.1.0-Release.6021101/dingtalk_dll.so
```

ubuntu 25.04 之后版本使用patchelf,因为execstack已经不能直接使用,具体命令如下：

```sh
# 1. 安装 patchelf
sudo apt install patchelf

# 2. 修复钉钉的动态库
sudo patchelf --clear-execstack /opt/apps/com.alibabainc.dingtalk/files/8.1.0-Release.6021101/dingtalk_dll.so

# 3. (可选) 检查修复结果
readelf -l /opt/apps/com.alibabainc.dingtalk/files/8.1.0-Release.6021101/dingtalk_dll.so | grep GNU_STACK
# 输出应为：GNU_STACK      0x000000 0x00000000 0x00000000 0x000000 RW  0x10
# 注意末尾是 "RW" 而不是 "RWE" (E 代表 Executable)
```

管理窗口重新启动，即可解决相关问题！
