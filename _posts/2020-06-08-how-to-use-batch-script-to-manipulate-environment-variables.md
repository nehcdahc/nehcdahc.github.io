---
layout: post
title: "如何使用 batch 脚本操作环境变量"
date: 2020-06-08 20:49:00 +0800
tags: [Batch]
categories: ["操作系统", "编程语言"]
---

```batch
rem local environment
reg delete "HKCU\Environment" /f /v {ENVIRONMENT_VARIABLE_NAME}

rem system environment
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /f /v {ENVIRONMENT_VARIABLE_NAME}

set emptyWord=

rem remove from local environment path
FOR /F "skip=2 tokens=2,*" %%A IN ('reg query "HKCU\Environment" /v "Path"') DO set "localPath=%%B"
call set newPath=%%localPath:%{OLD_PATH}%=%emptyWord%%%
reg add "HKCU\Environment" /f /v Path /t REG_SZ /d "%newPath%"

rem remove from system environment path
FOR /F "skip=2 tokens=2,*" %%A IN ('reg query "HKCU\Environment" /v "Path"') DO set "systemPath=%%B"
call set newPath=%%systemPath:%{OLD_PATH}%=%emptyWord%%%
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /f /v Path /t REG_SZ /d "%newPath%"
```

## 参考

- <https://www3.ntu.edu.sg/home/ehchua/programming/howto/Environment_Variables.html#:~:text=To%20list%20all%20the%20environment%20variables%2C%20use%20the%20command%20%22%20env,Windows%20uses%20%25varname%25%20).>
