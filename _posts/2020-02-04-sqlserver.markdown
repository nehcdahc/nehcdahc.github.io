---
layout: post
title:  "常用 SQL Server 脚本"
date:   2020-02-04 14:48:00 +0800
tags: [PostgreSQL, SQL, Database]
categories: ["数据库", "编程语言"]
---

## 运行维护

### 备份还原

```bash
# server_name
# user_name
# password
# database_name
# backup_path，备份路径，X:PathToBackupLocation[Name_of_Database].bak
SQLCMD -S {server_name} -U {user_name} -P {password} -d master -Q"BACKUP DATABASE {database_name} to disk='{backup_path}'"
```
