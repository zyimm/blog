---
title: 👣ssh登陆出现Too many authentication failures
date: 2024-12-13
tags: 
    - ssh
    - Linux
---

 最近遇到ssh登陆"Too many authentication failures" 错误时，这个错误是服务器为了防止暴力破解攻击，设置了最大认证尝试次数的限制。可是第一次登陆时候也会出现这个错误，原因是：

 1. 本地会根据/etc/ssh/ssh_config文件中的PreferredAuthentications尝试使用的身份验证方法的偏好顺序，进行多次验证。
 2. 我本地ssh有很多存在密钥，ssh登陆时候会依次用这些密钥进行尝试，直到成功为止。

上述原因导致ssh多次登陆失败从而触发服务器最大认证尝试次数的限制，抛出"Too many authentication failures"错误。

<!--more-->
`PreferredAuthentications`是ssh客户端的配置，通常在/etc/ssh/ssh_config文件中，用于指定ssh客户端尝试使用的身份验证方法的偏好顺序。ssh是支持多种认证方式，目前有如下常用配置：

1. publickey：使用公钥/私钥对进行认证。
2. password：通过输入用户密码进行认证。
3. keyboard-interactive：一种交互式认证方式，可以是密码或者其他形式的质询响应。
4. gssapi-with-mic：使用 GSSAPI (Generic Security Service Application Program Interface) 进行认证，通常与 Kerberos 一起使用。
5. hostbased：基于主机的身份验证，客户端机器本身被用来识别用户。

在`man ssh_config`中，通过查看PreferredAuthentications的说明，可知道默认的认证顺序为：

`gssapi-with-mic,hostbased,publickey,keyboard-interactive,password`

**修改方法如下：**

1. （**不建议）**服务器端的ssh配置文件 /etc/ssh/sshd_config，可以考虑增加 MaxAuthTries 的值来允许更多的认证尝试。修改之后需要重启ssh服务才能生效。
2. 调整客户端/etc/ssh/ssh_config文件中的PreferredAuthentications 将password优先级放在第一。

```sh
PreferredAuthentications password,publickey,gssapi-with-mic,hostbased,keyboard-interactive
```

>> 后期使用密钥进行免登录，可能需要将publickey设置优先级第一，以免密钥无法免登录。
