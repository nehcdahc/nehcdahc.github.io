---
layout: post
title: "如何进行 IIS Log 分析"
date: 2020-02-28 08:43:00 +0800
categories: ["工具"]
---

## 找到 IIS Log

1. [打开 Internet Information Service (IIS) Manager](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj635847(v%3Dws.11))
2. 点击左侧 Connections > Sites，在右侧 Sites 列表中定位到所要分析的站点，并记住其 ID
3. 双击站点，在右侧 Features View 找到 Logging 图标，并双击打开 Logging 页面，拷贝 Logging > Directory 的值（通常这个值为 %SystemDrive%\inetpub\logs\LogFiles），并在资源管理器中打开
4. 根据第二步记录的 ID，找到对应的网站日志文件文件夹

## 分析 IIS Log

### 利用 Log Parser Studio

- <https://gallery.technet.microsoft.com/office/Log-Parser-Studio-cd458765>

### 利用 WPS

1. 打开 WPS > Spreadsheets > Blank
1. 选择 Data > Import Data

   1. Select data source > Open data file directly (All Files(_._)) > Next
   1. File Conversion > Text encoding (Other encoding:) > Next
   1. Text Import Wizard - Step 1 of 3
      1. Original data type > Delimited
      1. Start import at row > 4
      1. Next
   1. Text Import Wizard - Step 2 of 3
      1. Delimiters > Tab & Space
      1. Text qualifier: "
      1. Next
   1. Text Import Wizard - Step 3 of 3 > Finish

1. 将第一行的标题修正正确 —— 多了 1 列 #Fields:

### 利用 Log Parser

1. 下载 [Log Parser 2.2](https://www.microsoft.com/en-us/download/details.aspx?id=24659)，并安装
1. 将 Log Parser 2.2 的执行目录 “C:\Program Files (x86)\Log Parser 2.2” 添加到环境变量
1. 使用下面脚本转换成 csv 文件

   ```powershell
   logparser -i:W3C -o:csv "SELECT * INTO c:\temp\results.csv FROM c:\temp\myLogFile.log"

   logparser.exe "SELECT * FROM u_ex17020200.log WHERE cs-uri-query LIKE '%updvmsisdn.aspx%'" -o:CSV -q:ON -stats:OFF >> C:\Log1\output.csv

   logparser.exe "SELECT * FROM u_ex*.log WHERE cs-uri-stem LIKE '%updvmsisdn.aspx%' AND cs-uri-query LIKE '%countryId=31%'" -o:CSV -q:ON -stats:OFF >> C:\Users\Administrator\Desktop\temp\output.csv

   logparser.exe "SELECT * FROM u_ex17022402.log WHERE cs-uri-stem LIKE '%updvmsisdn.aspx%' AND cs-uri-query LIKE '%countryId=31%'" -o:CSV -q:ON -stats:OFF >> C:\Users\Administrator\Desktop\temp\TEMP.csv
   ```

## 参考

- <https://gist.github.com/GuyHarwood/5540179>
- <https://gist.github.com/dejanstojanovic/30c757f0b093a9e24314e82a8abe98d2>
