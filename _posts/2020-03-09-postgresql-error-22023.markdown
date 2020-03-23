---
layout: post
title:  "SQL ERROR [22023]: ERROR: new collation is incompatible with the collation of the template database (English_United States.1252)"
date:   2020-03-09 13:21:00 +0800
tags: [PostgreSQL, SQL, Database]
categories: ["数据库", "编程语言"]
---

## 重现步骤

执行下面 SQL：

```sql
-- LC_COLLATE：string sort order
-- LC_CTYPE：character classification
-- database_name，数据库名称
-- database_user，用户名
CREATE DATABASE {database_name} WITH OWNER = {database_user} ENCODING 'UTF8' LC_COLLATE = 'Chinese (Simplified)_China.936' LC_CTYPE = 'Chinese (Simplified)_China.936';
```

发生错误：

SQL Error [22023]: ERROR: new collation (Chinese (Simplified)_China.936) is incompatible with the collation of the template database (English_United States.1252)
  Hint: Use the same collation as in the template database, or use template0 as template.

## 解决方案

使用 TEMPLATE 指定创建数据库的模板数据库

```sql
-- LC_COLLATE：string sort order
-- LC_CTYPE：character classification
-- database_name，数据库名称
-- database_user，用户名
CREATE DATABASE {database_name} WITH OWNER = {database_user} ENCODING 'UTF8' LC_COLLATE = 'Chinese (Simplified)_China.936' LC_CTYPE = 'Chinese (Simplified)_China.936' TEMPLATE template0;
```

## 参考

- <https://stackoverflow.com/questions/18870775/how-to-change-the-template-database-collection-coding>
