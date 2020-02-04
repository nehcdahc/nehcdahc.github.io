---
layout: post
title:  "常用 SQL Server 脚本"
date:   2020-02-04 14:48:00 +0800
---

## 运行维护

### 备份还原

```bash
SQLCMD -S server_name -U user_name -P password -d master -Q"BACKUP DATABASE database_name to disk=’X:PathToBackupLocation[Name_of_Database].bak'"
```
