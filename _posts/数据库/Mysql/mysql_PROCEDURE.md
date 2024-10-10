---
title: 🦻 MySQL的1449错误，definer不存在处理
date: 2024-10-10
tags: 
  - mysql
---

MySQL 错误 1449 通常表示 `The user specified as a definer ('user'@'host') does not exist`。出现这个错误的原因是mysql在尝试执行某个存储过程、函数或视图时，定义者（definer）指定的用户不存在于数据库中。

首先先查看一下，目前数据库中存储过程有哪些定义者，sql如下：

```sh
SELECT 
    ROUTINE_NAME, 
    DEFINER
FROM 
    information_schema.ROUTINES
WHERE 
    ROUTINE_TYPE = 'PROCEDURE' 
    AND ROUTINE_SCHEMA = 'your_database_name'; # your_database_name 替换成实际数据库名称
```
<!--more-->
看一下DEFINER的值，是否再目前数据库用户当中，根据实际情况进行如下1或2解决：

## 1. 创建缺失的用户

```sh
CREATE USER 'user'@'host' IDENTIFIED BY 'password';
```

替换 'user'、'host' 和 'password' 为实际值。之后别忘了赋予适当的权限给新创建的用户。

赋予权限如下：

```sh
GRANT SELECT, INSERT, UPDATE ON my_database.my_table TO 'user'@'host'; # my_database.my_table 赋予 SELECT, INSERT, UPDATE

#或赋予所有权限，视情况而定

GRANT ALL PRIVILEGES ON my_database.my_table TO 'user'@'host';

```

更改权限之后，需要刷新权限！

```sh
FLUSH PRIVILEGES; 
```

### 2. 修改定义者的身份

修改存储过程、函数或视图的定义者为一个存在的用户。

```sh
ALTER DEFINER = 'new_user'@'new_host' PROCEDURE your_procedure_name #'new_user'@'new_host' 替换实际用户;
```
