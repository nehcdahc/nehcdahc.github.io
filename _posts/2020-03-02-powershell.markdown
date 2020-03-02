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
