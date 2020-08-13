---
layout: post
title: "常用 PostgreSQL 脚本"
date: 2020-02-04 13:59:00 +0800
tags: [PostgreSQL, SQL, Database]
categories: ["数据库", "编程语言"]
---

## 数据定义

### 数据库

```sql
-- 创建数据库
-- https://www.postgresql.org/docs/current/static/multibyte.html
-- database_name，数据库名称
-- database_user，用户名
CREATE DATABASE {database_name} WITH OWNER = {database_user};
CREATE DATABASE {database_name} OWNER {database_user};

-- LC_COLLATE：string sort order
-- LC_CTYPE：character classification
-- database_name，数据库名称
-- database_user，用户名
CREATE DATABASE {database_name} WITH OWNER = {database_user} ENCODING 'UTF8' LC_COLLATE = 'zh_CN.UTF-8' LC_CTYPE = 'zh_CN.UTF-8' TEMPLATE = template0;
-- OR WINDOWS
CREATE DATABASE {database_name} WITH OWNER = {database_user} ENCODING 'UTF8' LC_COLLATE = 'Chinese (Simplified)_China.936' LC_CTYPE = 'Chinese (Simplified)_China.936' TEMPLATE = template0;

-- 复制数据库
-- database_name，数据库名称
-- database_user，用户名
-- original_database_name，原始数据库名称
CREATE DATABASE {database_name} WITH TEMPLATE {original_database_name} OWNER {database_user};

-- 重命名数据库
-- database_name，数据库名称
-- new_database_name，新的数据库名称
SELECT
    pg_terminate_backend (pid)
FROM
    pg_stat_activity
WHERE
    datname = '{database_name}';
ALTER DATABASE {database_name} RENAME TO {new_database_name};
```

### 表

```sql
-- 新增列
-- table_name，表名
-- column_name，列名
-- column_type，列类型
ALTER TABLE {table_name} ADD COLUMN IF NOT EXISTS {column_name} {column_type} [NULL | NOT NULL];
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
-- 创建索引
CREATE INDEX IF NOT EXISTS {index_name} ON {table_name} USING btree ({column_name});

-- Query the indexes of a table
-- table_name，表名
SELECT * FROM pg_indexes WHERE tablename IN ('{table_name}');

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

## 数据查询和操作

```sql
-- 检查不存在则写入
INSERT INTO {table_name}({column_name1} [, {column_name2}, ...])
SELECT {column_value1} [, {column_value2}, ...]
WHERE NOT EXISTS (
    SELECT 1 FROM {table_name} WHERE ...
)
```

## 权限控制

```sql
-- CREATE USER OR ROLE，PostgreSQL 中创建用户和角色是等效的
-- role_name，用户角色名称
-- user_password，用户密码
-- user_name，用户角色名称
CREATE ROLE {role_name} WITH CREATEDB CREATEROLE LOGIN PASSWORD '{user_password}';
CREATE user {user_name} PASSWORD '{user_password}';

-- 分配所有权限
-- database_name，数据库名称
-- database_user，数据库用户
GRANT ALL PRIVILEGES ON {database_name} TO {database_user};

-- 修改表的 Owner
-- table_name，表名
-- database_user，数据库用户
ALTER TABLE {table_name} OWNER TO {database_user};

-- 分配 FUNCTION 的权限给指定用户
-- function_name，函数名称
-- parameter1_type，第一个函数参数类型
-- parameter2_type，第二个函数参数类型
-- database_user，数据库用户
GRANT EXECUTE ON FUNCTION {function_name}([{parameter1_type}, {parameter2_type}, ...]) TO {database_user};

-- 修改 FUNCTION 的 Owner
-- function_name，函数名称
-- parameter1_type，第一个函数参数类型
-- parameter2_type，第二个函数参数类型
-- database_user，数据库用户
ALTER FUNCTION {function_name}([{parameter1_type}, {parameter2_type}, ...]) OWNER TO {database_user};
```

## 运行分析

```sql
-- 查询当前数据库 TOP 20 大表
SELECT table_name
    ,pg_size_pretty(pg_relation_size(table_schema || '.' || table_name)) AS size
FROM information_schema.tables
ORDER BY pg_relation_size(table_schema || '.' || table_name) DESC LIMIT 20;

-- 查询单个表大小
-- table_name，表名
SELECT pg_size_pretty(pg_relation_size({table_name}));

-- 查询数据库活动的查询
SELECT current_timestamp - query_start AS runtime
    ,query_start
    ,datname
    ,pid
    ,query
FROM pg_stat_activity
WHERE query_start IS NOT NULL
ORDER BY 1 DESC limit 20;

-- 查询系统中谁阻塞了谁
-- From：《PostgreSQL for Data Architects 数据架构师的 PostgreSQL 修炼》 Jayadevan Maymala 著 戚长松 译
SELECT
    waiting1.pid AS waiting_pid,
    waiting2.usename AS waiting_user,
    waiting2.query AS waiting_statement,
    blocking1.pid AS blocking_pid,
    blocking2.usename AS blocking_user,
    blocking2.query AS blocking_statement
FROM pg_locks AS waiting1
JOIN pg_stat_activity AS waiting2
    ON waiting1.pid = waiting2.pid
JOIN pg_locks AS blocking1
    ON waiting1.transactionid = blocking1.transactionid
        AND waiting1.pid != blocking1.pid
JOIN pg_stat_activity AS blocking2
    ON blocking1.pid = blocking2.pid
WHERE NOT waiting1.GRANTED;
```

## 运行维护

```sql
-- Check server version
SELECT VERSION();

-- Cancel Processes by pid
SELECT pg_cancel_backend(pid int);

-- Terminate Processes by pid
SELECT pg_terminate_backend(pid int);

-- Kill all existing connections in the original database
-- source_db，数据库名称
SELECT pg_terminate_backend(pg_stat_activity.pid)
FROM pg_stat_activity
WHERE pg_stat_activity.datname = '{source_db}'
AND pid <> pg_backend_pid();

-- garbage-collect and optionally analyze a database
-- table_name，数据库表名
VACUUM {table_name};
VACUUM FULL {table_name};

vacuumdb --help
vacuumdb --host={host} --username={user_name} --all --full
vacuumdb --host={host} --username={user_name} --all --full
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
# host_name，主机
# database_user，数据库用户
# file_path，备份文件路径
# database_name，数据库名称
# --blobs 在转储中包含大对象。除非指定了--schema, --table, --schema-only开关，否则这是默认行为。因此-b 开关仅用于在选择性转储的时候添加大对象。
# --verbose 指定冗余模式。这样将令pg_dump 输出详细的对象评注以及转储文件的启停时间和进度信息到标准错误上。
# --table=table
# --exclude-table=table
pg_dump --host {host_name} --port {port} --username {database_user} --format c --blobs --verbose --file {file_path} {database_name}
pg_dump --host {host_name} --port {port} --username {database_user} --format c --table={table1_name} --table={table2_name} --verbose --file {file_path} {database_name}

pg_restore --host {host_name} --port {port} --username {database_user} --no-owner --dbname {database_name} {file_path}
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
-- begin_time
-- end_time
SELECT EXTRACT(epoch FROM ({begin_time} - {end_time}));

-- Query the last month in format 'YYYYMM'
SELECT to_char(date_trunc('month', current_date - interval '1' month), 'YYYYMM');
```

### psql

```bash
# 打开数据库连接
# host_name
# database_user
psql -h {host_name} -U {database_user}

# 列出所有的数据库
\l

# 连接数据
# database_name
\c {database_name}
```

### File Locations

```sql
SHOW data_directory;
SHOW config_file;
SHOW hba_file;
```

## 修改记录

- 2020-08-13 新增了 vacuumdb 清理数据库的脚本
- 2020-07-16 新增了查询阻塞情况的脚本
- 2020-06-12 新增了 pg_dump 指定表的脚本
- 2020-06-03 新增修改数据库名的 SQL
- 2020-04-28 新增查询服务器版本的 SQL
- 2020-03-23 新增数据查询和操作的 SQL
- 2020-03-17 新增 File Locations 节点
- 2020-03-12 修改备份脚本：增加了 port 参数；将缩写命令改成完整参数命令，便于阅读
