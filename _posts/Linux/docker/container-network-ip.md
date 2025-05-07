---
title: 🫡Docker运行一个容器并固定IP
date: 2023-8-07
tags: 
  - Docker
---

一些特殊部署场景下，需要指定某个容器ip以便于访问。可以使用Docker的网络模式来间接地控制容器的IP地址，所以这边记录一下分别在单独docker与docker-compose配置示例！☘

## docker

### 1.自定义一个名为my-docker-net网络

```sh
docker network create --subnet=172.0.0.0/24 my-docker-net
```

### 2.指定容器网络并设置固定ip

```sh
docker run -d   --net=my-docker-net --ip=172.0.0.3 --name=[容器名字] [镜像名字]
```

`--net` 指定网络  
`--ip`  指定ip  

通过IP`172.0.0.3`访问该容器了。

## docker-compose

在docker-compose.yml文件中`networks`节点进行配置

### 1.subnet 进行配置网段

```yml
networks:
  default:
    driver: bridge
    ipam:
      driver: default
      # 解除下面的注释可以设置网段，用于nginx等容器固定容器IP
      config:
        - subnet: 172.0.0.0/24
```

### 2.对应service下`networks`节点进行配置固定ip

```yml
version: "3"
    services:
        nginx:
            networks:
                default:
                    ipv4_address: 172.0.0.3
```

以上命令对compose的网络配置一个172.0.0.0/24网段，然后指定其中`nginx`指定ip为`172.0.0.3`

<!--more-->
## 补充知识

172.0.0.0/24 是一个 CIDR（Classless Inter-Domain Routing）表示法，它使用网络地址和掩码长度来表示一个 IP 地址范围。255.255.255.0 是对应的子网掩码，也称为网络掩码。包含了从 172.0.0.0 到 172.0.0.255 的所有 IP 地址。

在 CIDR（Classless Inter-Domain Routing）表示法中，斜线后的数字表示掩码长度，用于指示网络地址中的连续前缀位数。

具体来说，掩码长度是指网络地址中左边的连续位数，用于表示网络部分。剩余的位数则用于表示主机部分。

对于 IPv4 地址，掩码长度的有效范围是从 0 到 32。较小的掩码长度表示更大的网络范围，而较大的掩码长度表示更小的网络范围。

例如：

1. /8 表示网络地址中的前 8 位用于网络部分，后 24 位用于主机部分。
2. /16 表示网络地址中的前 16 位用于网络部分，后 16 位用于主机部分。
3. /24 表示网络地址中的前 24 位用于网络部分，后 8 位用于主机部分。

> 因此`/8` 中的数字 8 表示网络地址中的前 8 位用于网络部分，剩下的位用于主机部分。这意味着 172.0.0.0/8 表示一个非常大的网络范围，其中 172.0.0.0 到 172.255.255.255 都属于同一个网络。
