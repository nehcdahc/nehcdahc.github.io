---
layout: post
title: "%cd% 与 %~dp0% 的区别"
date: 2020-03-02 18:04:00 +0800
tags: [Batch]
categories: ["操作系统", "编程语言"]
---

1. 使用范围

   - %cd%：.bat file、cmd
   - %~dp0%: .bat file

1. 示例脚本

   1. 创建文件 D:\cd~dp0.bat

      ```batch
      @echo off
      echo this is %%cd%% : %cd%
      echo this is %%~dp0 : %~dp0
      ```

   1. 在 C:\Users\Administrator 路径执行

      ```
      this is %cd% : C:\Users\Administrator
      this is %~dp0 : D:\
      ```

   1. 在 D:\ 路径执行

      ```
      this is %cd% : D:\
      this is %~dp0 : D:\
      ```

## 参考

- [bat 中%cd%和%~dp0 的区别]<https://blog.csdn.net/lolichan/article/details/84919982>
