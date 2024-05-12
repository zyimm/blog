---
title: "mysqldump: Couldn't execute 'SELECT COLUMN_NAME...的问题处理"
date: 2023-05-20
tags: mysql
---

## 解决

在日常使用mysqldump进行数据备份时，会出现如下的问题：

```sh
mysqldump -h ***  -u root -p --skip-lock-tables mig_test > /var/log/mysql/bak.sql
Enter password:
#输入密码之后报如下错误：
mysqldump: Couldn't execute 'SELECT COLUMN_NAME, 
    JSON_EXTRACT(HISTOGRAM, '$."number-of-buckets-specified"')  
    FROM information_schema.COLUMN_STATISTICS                
    WHERE SCHEMA_NAME = 'mig_test' AND TABLE_NAME = 'migrations';':

Unknown table 'COLUMN_STATISTICS' in information_schema (1109)
```
经查阅相关资料得知，需要添加`--column-statistics=0`即可。我猜测的原因可能是备份数据时候在读取相关表结构没有相关权限，所以报此错误。通过添加`--column-statistics=0`进行关闭，以免报错。

<!--more-->
## column-statistics解释

`--column-statistics` 是一个用于输出列统计信息的选项。这个选项可以让 mysqldump 在备份数据时，输出每个表的列名、数据类型、长度、默认值、是否为 null 等信息，同时还输出了每个列的统计信息，包括平均长度、总长度、字符集、数字类型的比例等信息。

列统计信息对于优化备份文件的大小和速度非常有用。例如，如果你知道某个列中大多数数据都是 NULL，那么你可以减少备份该列的数据，从而减少备份文件的大小。另外，如果你知道某个列的数据类型和长度，你可以更好地规划备份文件的大小和格式，以最大化备份效率。


