---
layout: post
title:  "如何解决 Windows 10 上安装 PostgreSQL 日志乱码的问题"
date:   2020-05-27 11:44:00 +0800
tags: [PostgreSQL, SQL, Database]
categories: ["数据库", "编程语言"]
---

## 日志乱码

```
2020-05-26 02:02:05.729 HKT [8100] 鏃ュ織:  鐢变簬缁熻鏀堕泦鍣ㄦ棤鍝嶅簲鑰屼娇鐢ㄦ棫鐨勭粺璁′俊鎭潵浠ｆ浛褰撳墠鐨勭粺璁′俊鎭?
2020-05-26 02:30:05.439 HKT [3152] 閿欒:  鍦ㄥ瓧娈?"ownerid" 涓┖鍊艰繚鍙嶄簡闈炵┖绾︽潫
```

## 解决方案

1. 打开配置文件 ~\data\postgresql.conf
1. 修改 lc_messages = 'C'