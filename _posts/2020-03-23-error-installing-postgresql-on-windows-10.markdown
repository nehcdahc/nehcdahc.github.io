---
layout: post
title:  "在 Windows 10 上安装 PostgreSQL 时出错：Warning：Problem running post-install step. Installation may not complete correctly
Failed to start the database server."
date:   2020-03-23 09:24:00 +0800
tags: [PostgreSQL, SQL, Database]
categories: ["数据库", "编程语言"]
---

## 错误信息

Warning
Problem running post-install step. Installation may not complete correctly
Failed to start the database server.

## 解决方案

### 方案 1（推荐）

安装 PostgreSQL 到 WSL 上，参考 <https://gist.github.com/michaeltreat/40a2f444d8ff6c89af958733448da093>

### 方案 2

手动创建 postgres 用户，然后执行安装操作。具体步骤如下：

1. 删除已经安装的 PostgreSQL，删除安装目录的文件

1. 检查是否有 postgres 用户，如果有，则删除 postgres 用户。或者执行下面的脚本直接删除

```powershell
net user postgres /delete
```

1. 创建 postgres 用户，并加入到 Administrators 和 power user 组

```powershell
net user /add postgres <password>

net localgroup administrators postgres /add

net localgroup "power users" postgres /add
```

1. 使用 postgres 用户运行 cmd.exe

```bat
runas /use:postgres cmd.exe
```

1. 执行安装

```bat
.\postgresql-12.x.x-windows-x64.exe
```

1. 从 Administrators 用户组中移除 postgres 用户

```powershell
net localgroup administrators postgres /delete
```

## 参考

- [Install psql on WSL](https://gist.github.com/michaeltreat/40a2f444d8ff6c89af958733448da093)
- [Installation Instructions for WSL 2](https://docs.microsoft.com/en-us/windows/wsl/wsl2-install)
