---
title: 👣ssh 免密登陆
date: 2022-10-06
tags: 
    - ssh
    - Linux
---

由于更换新电脑，需要生成新的密钥去免密登陆服务器，小记一下流程，免得下次还得搜索一下相关教程。

## 密钥登录的过程

SSH 密钥登录分为以下的步骤。

1. 客户端通过ssh-keygen生成自己的公钥和私钥。
2. 手动将客户端的公钥放入远程服务器的指定位置。
3. 客户端向服务器发起 SSH 登录的请求。
4. 服务器收到用户 SSH 登录的请求，发送一些随机数据给用户，要求用户证明自己的身份。
5. 客户端收到服务器发来的数据，使用私钥对数据进行签名，然后再发还给服务器。
6. 服务器收到客户端发来的加密签名后，使用对应的公钥解密。若解密后数据一致，则允许用户登录。

**校验过程时序图**

![25dd3fc40a001c3002872d39865e4baa](https://www.zyimm.com/images/media/20231227/25dd3fc40a001c3002872d39865e4baa.png)

<!--more-->
## 生成公/私密钥

```shell
# 我这边是linux windows 系统可以在当前用户文件目录下自行创建该文件夹
cd ~/.ssh 
# 生成密钥 可使用-t参数，指定密钥的加密算法。默认RSA 其他算法可以搜索了解一下 然后一路回车即可
# 中间有询问是否要为私钥文件设定密码保护（passphrase）相当于二次密码保护默认可以不需要
ssh-keygen

```

## 上传公钥到服务器上

生成密钥以后，公钥必须上传到服务器，才能使用公钥登录。公钥是以.pub 结尾不要和私钥搞混了。

一般我习惯手动上传公钥到服务器`~/.ssh/authorized_keys`文件。这里需要注意你要以哪个用户的身份登录到服务器，密钥就必须保存在该用户主目录的`~/.ssh/authorized_keys` 中。

手动上传就是直接将公钥里面文本直接追加到`~/.ssh/authorized_keys`里面。

另外还可以使用`ssh-copy-id` 命令进行自动上传，本质就是自动把公钥里面文本直接追加到`~/.ssh/authorized_keys`里面。密令如下：

```sh
ssh-copy-id -i pub_key_file user@host
```

上面命令中，-i参数用来指定公钥文件，user是所要登录的账户名，host是服务器地址。如果省略用户名，默认为当前的本机用户名。执行完该命令，公钥就会拷贝到服务器。

## 配置config

配置config目的在于简化ssh登录命令，在当前用户 `~/.ssh/` 新建config文本 若存在无需创建。

配置如下：

```sh
Host zyimm
    HostName 192.168.1.1
    User tom
    Port 22
    IdentityFile file_key
```

> 配置文本格式，只需要同级别空格对齐即可。

**HostName**

需要ssh连接过去的主机名，一般是IP地址。

**User**

登录主机的用户名

**IdentityFile**

认证证书文件，默认位置是~/.ssh/id_rsa, ~/ssh/id_dsa等，如果采用默认的证书，可以不用设置此参数，除非你的证书放在某个自定义的目录，那么你就需要设置该参数来指向你的证书

**Port**

SSH访问主机的端口号，默认是22端口，同上，只有在非默认情况下才需要设置该值

## 使用

```sh
ssh  zyimm # 即可免密登陆192.168.1.1服务器了
```

如果使用vscode 建议下载 Remote-ssh 扩展,搭配使用更舒服！

> 记得配置之后需要重启服务器上的ssh服务！常见重启ssh服务命令如下（选择其中之一即可）：

```sh
service sshd restart
systemctl restart sshd.service
/etc/init.d/ssh restart
```

## 调试

如果在上面步骤操作完之后，仍然出现一些问题，可以在命令中带上-v以便定位问题

```sh
ssh  zyimm  -v
```

## 登录很慢

如果使用ssh连接Linux服务器，登录很慢可能会等待一段时间，这是由于服务器的DNS解析问题或gssapi认证导致的。可以使用以下办法解决：

1.DNS反向解析问题

OpenSSH在用户登录的时候会验证IP，它根据用户的IP使用反向DNS找到主机名，再使用DNS找到IP地址，最后匹配一下登录的IP是否合法。如果客户机的IP没有域名，或者DNS服务器很慢或不通，那么登录就会很花时间。

解决办法：在目标服务器上修改sshd服务器端配置,并重启sshd

```sh
vi /etc/ssh/sshd_config
UseDNS no
```

2.关闭ssh的gssapi认证

用ssh -v user@server 可以看到登录时有如下信息：

```sh
debug1: Next authentication method: gssapi-with-mic
debug1: Unspecified GSS failure. Minor code may provide more information
```

解决办法：

修改sshd服务器端配置

```sh
GSSAPIAuthentication no
```

GSSAPI ( Generic Security Services Application Programming Interface) 是一套类似Kerberos 5的通用网络安全系统接口。
