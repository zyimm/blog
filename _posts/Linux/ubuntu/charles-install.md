---
title: Ubuntu 22.04.1下安装Charles
date: 2022-11-05
tags: Charles
---

[charles 官网](https://www.charlesproxy.com/) 下载Charles，Debian系列发行版可以使用 `apt-get install charles-proxy` 安装,不过需要提前安装对应key才可以安装，我这边是直接下载安装包解压安装。


## 解压&安装
```
# 下载包具体以最新版本为准
tar -xvf charles-proxy-4.6.3_amd64.tar.gz

# 移动到程序安装目录
mv charles /opt/

# 检查一下 charles/bin/charles 是否具有执行权限没有需要`chmod u+x`

```

## 创建启动图标
因为解压安装默认是不会生成启动图标的，所以需要单独创建

进入 `/usr/share/applications` 目录下

```
cd /usr/share/applications

sudo touch charles.desktop

sudo gedit charles.desktop

```

接着输入如下信息:
```sh
[Desktop Entry]
Name=Charles
Exec=/opt/charles/bin/charles
Terminal=false
Type=Application
Icon=/opt/charles/icon/512x512/apps/charles-proxy.png
StartupWMClass=Charles
Comment=Charles
Categories=Utility;

```
说明一下：
1. Exec 是程序执行路径
2. Icon 图标
3. Name 程序名称


其他配置说明自行百度～

`sudo chmod u+x charles.desktop` 最后赋予执行权限，接下来在启动页面搜索`Charles` 可以启动对应程序！



## Charles激活

### 现成激活账号

```
https://zhile.io

48891cf209c6d32bf4

```



### Charles在线免费工具生成一个新的注册码

[破解网址:www.zzzmode.com](https://www.zzzmode.com/mytools/charles/)


然后打开Charles，在上方菜单栏的“Help”选择“Register Charles”输入生成账号和注册码即可


## 参考
1. [知乎：Charles安装与激活](https://zhuanlan.zhihu.com/p/537292816)