---
title: 🐿mysql按照正则匹配,进行批量删除数据表
date: 2023-08-02
tags: mysql
---
有时候遇到批量删除很多数据表的时候，一个一个去删除太麻烦。使用如下sql可以根据则匹配,进行批量删除数据表！

注意删除数据表是很危险的行为，确保你确认所删除数据已经备份或确认无用！！！

跑路删库/表，是违法行为！！！🧟‍♀️

```sql
SET @query = '';

SELECT REPLACE(GROUP_CONCAT('DROP TABLE IF EXISTS `', TABLE_NAME, '`;'), ';,', ';') INTO @query
FROM information_schema.tables
WHERE table_schema = 'database_name'
  AND table_name REGEXP 'regex_pattern';
 PREPARE stmt FROM @QUERY;

EXECUTE stmt;

DEALLOCATE PREPARE stmt;

```
<!--more-->

上述代码中的 `database_name` 替换为要操作的数据库的名称，`regex_pattern` 替换为要匹配的表名的正则表达式模式。

此脚本将首先使用 SHOW TABLES 语句和 REGEXP 运算符来获取匹配正则表达式模式的表名。然后，它将使用 DROP TABLE 语句生成一个包含所有匹配表名的脚本，并使用 EXECUTE 执行删除操作！

**如果想确保万无一失的话，可以使用下面sql进行与生成drop语句预览以便再次确认！**
```sql
SELECT REPLACE(GROUP_CONCAT('DROP TABLE IF EXISTS `', TABLE_NAME, '`;'), ';,', ';')
FROM information_schema.tables
WHERE table_schema = 'database_name'
  AND table_name REGEXP 'regex_pattern';

```

>> 🚩 跑路删库/表，是违法行为！！！使用改sql进行违法，本人概不负责！！！