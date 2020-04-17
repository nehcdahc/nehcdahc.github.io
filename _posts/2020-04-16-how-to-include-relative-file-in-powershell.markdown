---
layout: post
title: "如何引用相对路径的 PowerShell 脚本文件"
date: 2020-04-16 17:23:00 +0800
tags: [PowerShell]
categories: ["操作系统"]
---

假设我们有两个 PowerShell 脚本文件，其中一个需要引用另外一个：

1. scripts\help.ps1
1. scripts\publish.ps1

第二个脚本 scripts\publish.ps1 调用第一个脚本 scripts\help.ps1，内容如下：

```powershell
. ".\help.ps1" # 关于 . 操作符，可以参考这里 https://docs.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_operators?view=powershell-7#dot-sourcing-operator-
# ...
```

如果在 scripts 目录下运行 publish.ps1，是没有问题的。但是如果我们在项目根目录使用 scripts\publish.ps1 运行，则会报脚本文件找不到。

解决方案，修改 publish.ps1 的内容为：

```powershell
. "$PSScriptRoot\help.ps1"
# ...
```

##　参考

- <https://docs.microsoft.com/zh-cn/powershell/module/microsoft.powershell.core/about/about_operators>
