---
layout: post
title:  "常用 PostgreSQL 脚本"
date:   2020-02-04 13:59:00 +0800
---

## 数据定义

### 数据库

```sql

-- 创建数据库
-- https://www.postgresql.org/docs/current/static/multibyte.html
-- database_name，数据库名称
-- database_user，用户名
CREATE DATABASE database_name WITH OWNER = database_user;
CREATE DATABASE database_name OWNER database_user;

-- LC_COLLATE：string sort order
-- LC_CTYPE：character classification
CREATE DATABASE database_name WITH OWNER = database_user ENCODING 'UTF8' LC_COLLATE = 'zh_CN.UTF-8' LC_CTYPE = 'zh_CN.UTF-8';
-- OR WINDOWS
CREATE DATABASE database_name WITH OWNER = database_user ENCODING 'UTF8' LC_COLLATE = 'Chinese (Simplified)_China.936' LC_CTYPE = 'Chinese (Simplified)_China.936';

-- 复制数据库
-- database_name，数据库名称
-- database_user，用户名
-- original_database_name，原始数据库名称
CREATE DATABASE database_name WITH TEMPLATE original_database_name OWNER database_user;

```

### 表

```sql

-- 新增列
-- table_name，表名
-- column_name，列名
ALTER TABLE table_name ADD COLUMN IF NOT EXISTS column_name VARCHAR(100) NULL;

```

### 扩展

```sql

-- 创建 UUID 扩展
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- 验证 UUID 扩展
SELECT uuid_generate_v4();

-- 创建 cube 扩展
CREATE EXTENSION IF NOT EXISTS cube;

-- 创建 earthdistance 扩展
CREATE EXTENSION IF NOT EXISTS earthdistance;

```

### 函数

```sql

-- 隐式将整形转换成字符串，但是会有一些问题，参考 https://stackoverflow.com/questions/50025750/postgres-convert-integer-into-text。通常情况下还是建议使用 CAST 函数来实现。
-- 使用场景：在数据库迁移的时候（比如 Microsoft SQL Server 转成 PostgreSQL，Microsoft SQL Server 默认是支持的）需要隐式转换，以达到快速实现的目的
CREATE FUNCTION pg_catalog.text(integer) RETURNS text STRICT IMMUTABLE LANGUAGE SQL AS 'SELECT textin(int4out($1));';
CREATE CAST (integer AS text) WITH FUNCTION pg_catalog.text(integer) AS IMPLICIT;
COMMENT ON FUNCTION pg_catalog.text(integer) IS 'convert integer to text';
CREATE FUNCTION pg_catalog.text(bigint) RETURNS text STRICT IMMUTABLE LANGUAGE SQL AS 'SELECT textin(int8out($1));';
CREATE CAST (bigint AS text) WITH FUNCTION pg_catalog.text(bigint) AS IMPLICIT;
COMMENT ON FUNCTION pg_catalog.text(bigint) IS 'convert bigint to text';

```

### 索引

```sql

-- Query the indexes of a table
SELECT * FROM pg_indexes WHERE tablename IN ('table_name');

-- 查询所有索引
SELECT
    i.relname AS indname ,
    i.relowner AS indowner ,
    idx.indrelid::REGCLASS ,
    am.amname AS indam ,
    idx.indkey ,
    ARRAY(
    SELECT
        pg_get_indexdef(idx.indexrelid,
        k + 1,
        TRUE)
    FROM
        GENERATE_SUBSCRIPTS(idx.indkey, 1) AS k
    ORDER BY
        k) AS indkey_names ,
    idx.indexprs IS NOT NULL AS indexprs ,
    idx.indpred IS NOT NULL AS indpred
FROM
    pg_index AS idx
JOIN pg_class AS i ON
    i.oid = idx.indexrelid
JOIN pg_am AS am ON
    i.relam = am.oid
JOIN pg_namespace AS ns ON
    ns.oid = i.relnamespace
    AND ns.nspname = ANY (CURRENT_SCHEMAS(FALSE));

-- 查询所有索引，排除系统表
SELECT
    U.usename AS user_name,
    ns.nspname AS schema_name,
    idx.indrelid :: REGCLASS AS table_name,
    i.relname AS index_name,
    idx.indisunique AS is_unique,
    idx.indisprimary AS is_primary,
    am.amname AS index_type,
    idx.indkey,
    ARRAY(
    SELECT
        pg_get_indexdef(idx.indexrelid,
        k + 1,
        TRUE)
    FROM
        GENERATE_SUBSCRIPTS(idx.indkey, 1) AS k
    ORDER BY
        k ) AS index_keys,
    (idx.indexprs IS NOT NULL)
    OR (idx.indkey::INT[] @> ARRAY[0]) AS is_functional,
    idx.indpred IS NOT NULL AS is_partial
FROM
    pg_index AS idx
JOIN pg_class AS i ON
    i.oid = idx.indexrelid
JOIN pg_am AS am ON
    i.relam = am.oid
JOIN pg_namespace AS NS ON
    i.relnamespace = NS.OID
JOIN pg_user AS U ON
    i.relowner = U.usesysid
WHERE
    NOT nspname LIKE 'pg%';

```

## 权限控制

```sql

-- CREATE USER OR ROLE，PostgreSQL 中创建用户和角色是等效的
-- role_name，用户角色名称
-- user_password，用户密码
-- user_name，用户角色名称
CREATE ROLE role_name WITH CREATEDB CREATEROLE LOGIN PASSWORD 'user_password';
CREATE user user_name PASSWORD 'user_password';

-- 分配所有权限
-- database_name，数据库名称
-- database_user，数据库用户
GRANT ALL PRIVILEGES ON database_name TO database_user;

-- 修改表的 Owner
ALTER TABLE table_name OWNER TO database_user;

-- 分配 FUNCTION 的权限给指定用户
-- function_name，函数名称
-- parameter1_type，第一个函数参数类型
-- parameter2_type，第二个函数参数类型
-- database_user，数据库用户
GRANT EXECUTE ON FUNCTION function_name(parameter1_type, parameter2_type, ...) TO database_user;

-- 修改 FUNCTION 的 Owner
-- function_name，函数名称
-- parameter1_type，第一个函数参数类型
-- parameter2_type，第二个函数参数类型
-- database_user，数据库用户
ALTER FUNCTION function_name(parameter1_type, parameter2_type, ...) OWNER TO database_user;

```

## 运行分析

```sql

-- 查询当前数据库 TOP 20 大表
SELECT table_name
    ,pg_size_pretty(pg_relation_size(table_schema || '.' || table_name)) AS size
FROM information_schema.tables
ORDER BY pg_relation_size(table_schema || '.' || table_name) DESC LIMIT 20;

-- 查询单个表大小
SELECT pg_size_pretty(pg_relation_size(table_name));

-- 查询数据库活动的查询
SELECT current_timestamp - query_start AS runtime
    ,query_start
    ,datname
    ,pid
    ,query
FROM pg_stat_activity
WHERE query_start IS NOT NULL
ORDER BY 1 DESC limit 20;

```

## 运行维护

```sql

-- Cancel Processes by pid
SELECT pg_cancel_backend(pid int);

-- Terminate Processes by pid
SELECT pg_terminate_backend(pid int);

-- Kill all existing connections in the original database
-- source_db，数据库名称
SELECT pg_terminate_backend(pg_stat_activity.pid)
FROM pg_stat_activity
WHERE pg_stat_activity.datname = 'source_db'
AND pid <> pg_backend_pid();

-- garbage-collect and optionally analyze a database
-- table_name，数据库表名
VACUUM table_name;
VACUUM FULL table_name;

```

### 配置

```sql

-- 修改 max_locks_per_transaction
ALTER SYSTEM SET max_locks_per_transaction = 300;

-- 重载配置信息，使配置生效
-- pg_hba.conf
SELECT pg_reload_conf();

```

### 备份还原

```bash

pg_dump -h host_name -U database_user -F c -b -v -f file_path database_name

pg_restore -h host_name -U database_user --no-owner -d database_name file_path

```

## 其他

```sql

-- Prepare a statement for execution
PREPARE foo(TEXT, TEXT, TEXT) AS
SELECT *
FROM foobar
WHERE foo = $1
    AND bar = $2
    OR baz = $3
EXECUTE foo('foo', 'bar', 'baz');
DEALLOCATE foo;

```

### 时间处理

```sql

-- 查询时间差
SELECT EXTRACT(epoch FROM (begin_time - end_time));

-- Query the last month in format 'YYYYMM'
SELECT to_char(date_trunc('month', current_date - interval '1' month), 'YYYYMM');

```

### psql

```bash

# 打开数据库连接
psql -h host_name -U database_user

# 列出所有的数据库
\l

# 连接数据
\c database_name

```
