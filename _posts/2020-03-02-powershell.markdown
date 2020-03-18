---
layout: post
title: "常用 Windows PowerShell 脚本"
date: 2020-03-02 18:04:00 +0800
tags: [PowerShell]
categories: ["操作系统"]
---

## 运算符

```powershell
# 小于
if ((Get-Date).minute -lt 30) {
    # ...
}
```

## 逻辑分支

```powershell
# if...else...
if ((Get-Date).minute -lt 30) {
    # ...
}
else {
    # ...
}
```

## 日期处理

```powershell
# 取分钟部分
(Get-Date).minute

# 格式化
Get-Date -Format 'HH31'
```

## 参数

```powershell

# download.ps1
Param (
    [Parameter(Mandatory = $true)]
    [String]
    $FileName,
    [Parameter(Mandatory = $true)]
    [Uri[]]
    $Addresses
)

for ($i = 0; $i -lt $Addresses.Count; $i++) {
    #
    # ...
    #
}

# build.ps1
$xtcUris = "http://...", "https://..."
.\download.ps1 -FileName $xtc -Addresses $uris
```

## 服务

```powershell
# 停止服务
Stop-Service -Name "{service_name}"

# 查看服务状态
Get-Service -Name "{service_name}"
```

## 修改记录

- 2020-03-18 08:18 添加管理 Windows 服务的脚本
