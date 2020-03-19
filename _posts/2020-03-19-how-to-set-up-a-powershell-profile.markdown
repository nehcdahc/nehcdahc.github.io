---
layout: post
title: "如何通过设置 PowerShell 配置文件提升工作效率"
date: 2020-03-19 15:25:00 +0800
tags: [PowerShell]
categories: ["操作系统"]
---

很多计算机日常任务我们都可以通过配置 PowerShell 的配置文件，达到快速执行的效果。比如：拷贝文件到一个特定的目录、定位到特殊的文件目录、执行一个上传 FTP 的任务等等。

## 配置步骤

1. 在 Windows PowerShell 提示窗口，使用下面的命令测试是否已经存在配置文件

   ```powershell
   Test-Path $profile
   ```

1. 如果上一步返回的结果不是 True，则应该执行下面的命令创建 profile 文件；否则跳过本步骤

   ```powershell
   New-item –type file –force $profile
   ```

1. 执行命令，使用文本编辑器打开 profile 文件

   ```powershell
   Code $profile # 使用 Visual Studio Code 打开
   # 或者
   Notepad $profile # 使用记事本打开
   ```

1. 修改配置文件内容，添加代码块

   示例：定义一个快速执行 git 的拉取和推送操作的命令

   ```powershell
   # 定义一个函数用来执行 git 的拉取和推送操作
   Function GitPullThenPush {
      git pull;
      git push;
   }

   # 给函数 GitPullThenPush 设定一个较短的别名
   Set-Alias gitpp GitPullThenPush
   ```

1. 保存并关闭文件

1. 重新打开 PowerShell 提示窗口，执行上面定义好的命令

   ```powershell
   gitpp # 同 git pull; git push;
   ```

## 注意事项

作为提醒，要允许执行一个未签名的脚本，必须执行下面的命令

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

在 Windows Server 2012 R2 及以上的操作系统版本中，这是一个默认选项。如果需要查看更多关于执行策略的信息，请执行 Get-Help 命令。

## 参考

- [How to set up a PowerShell Profile](https://blog.cloudbusiness.com/hub/how-to-set-up-a-powershell-profile)
- [timsneath/profile.ps1](https://gist.github.com/timsneath/19867b12eee7fd5af2ba)
