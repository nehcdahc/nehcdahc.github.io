---
layout: post
title: "如何默认以管理员身份运行 bat 文件"
date: 2020-06-08 20:42:00 +0800
tags: [Batch]
categories: ["操作系统", "编程语言"]
---

## 如果需要以管理员身份运行 bat 文件

1. 检查权限，无权限退出

   ```batch
   rem 检查是否使用管理员权限
   echo Administrative permissions required. Detecting permissions...

   net session >nul 2>&1
   if %errorLevel% == 0 (
      echo Success: Administrative permissions confirmed.
   ) else (
      echo Failure: Current permissions inadequate.
      goto end
   )
   ```

1. elevate to admin and also stay in the correct directory. <https://stackoverflow.com/questions/6811372/how-to-code-a-bat-file-to-always-run-as-admin-mode>

   ```batch
   set "params=%*"
   cd /d "%~dp0" && ( if exist "%temp%\getadmin.vbs" del "%temp%\getadmin.vbs" ) && fsutil dirty query %systemdrive% 1>nul 2>nul || (  echo Set UAC = CreateObject^("Shell.Application"^) : UAC.ShellExecute "cmd.exe", "/k cd ""%~sdp0"" && %~s0 %params%", "", "runas", 1 >> "%temp%\getadmin.vbs" && "%temp%\getadmin.vbs" && exit /B )
   ```
