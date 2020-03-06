---
layout: post
title: "如果修改 Windows 上安装的 PostgreSQL 的 PGDATA 目录"
date: 2020-03-06 18:30:00 +0800
tags: [PostgreSQL, Database]
categories: ["数据库", "编程语言"]
---

## 步骤

1. 停止 PostgreSQL 服务
1. 移动 PGDATA 目录到新的位置，该操作可能需要 Administrator 权限
1. 修改新的 PGDATA 目录权限，添加用户“Network Service”
1. 修改注册表，更新 PostgreSQL 服务指向的 PGDATA 路径。注册表路径为：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\pgsql-some version，修改 “ImagePath” 的 “-D” 为新的 PGDATA 目录。最后检查下 PostgreSQL 服务指向的 PGDATA 执行路径是否为新的路径。
   > 如果这里没有权限修改注册表，可以通过修改 PostgreSQL 服务的 command 执行路径
1. 重新启动 PostgreSQL 服务

## 参考

- [Change the default PGDATA directory on Windows](https://wiki.postgresql.org/wiki/Change_the_default_PGDATA_directory_on_Windows)
- [How to Migrate your PostgreSQL Data Directory in Windows](https://radumas.info/blog/tutorial/2016/08/08/Migrating-PostgreSQL-Data-Directory-Windows.html)
