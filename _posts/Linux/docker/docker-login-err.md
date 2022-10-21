---
title: docker login 失败问题解决
date: 2022-10-19 23:10:09
tags: Docker
---

## 问题

使用阿里云镜像服务进行 docker login 发生错误，具体如下：


```
docker login失败：err: exit status 1, Try “pass init“
```

后来搜索一下，找到问题解决办法！

## 解决办法



### 1.安装 docker-credential-pass
```
wget https://github.com/docker/docker-credential-helpers/releases/download/v0.6.0/docker-credential-pass-v0.6.0-amd64.tar.gz

tar -xf docker-credential-pass-v0.6.0-amd64.tar.gz

mv docker-credential-pass /usr/bin/.

docker-credential-pass //出现 You should see: "Usage: docker-credential-pass <store|get|erase|list|version>".

```

### 2.安装 gpg pass & 生成key
```
apt install gpg pass

# 下一步 生成key
gpg --generate-key #需要要填入姓名邮箱等信息. 输入新的password之后会产生一个新的key

# 复制上一步生成key
pass init (paste from clipboard) //直接从复制版上粘贴

```
### 3.设置密码
```
pass insert docker-credential-helpers/docker-pass-initialized-check # 输入新密码
```


### 4. 修改~/.docker/config.json 没有就创建
```json
{
    "credsStore":"pass" 
}
```

再次 docker login应该不会出现授权错误

## 参考链接：

[https://github.com/docker/docker-credential-helpers/issues/102](https://github.com/docker/docker-credential-helpers/issues/102)

