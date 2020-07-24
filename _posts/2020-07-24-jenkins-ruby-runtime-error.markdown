---
layout: post
title: "Jenkins 安装 ruby-runtime 出错"
date: 2020-07-24 17:38:00 +0800
categories: ["软件工程"]
tags: [Jenkins, CICD]
---

## C:\Program Files (x86)\Jenkins\plugins\ruby-runtime\WEB-INF\lib\classes.jar: The process cannot access the file because it is being used by another process.

Also:   java.nio.file.FileSystemException: C:\Program Files (x86)\Jenkins\plugins\ruby-runtime\WEB-INF\lib\classes.jar: 另一个程序正在使用此文件，进程无法访问。

at sun.nio.fs.WindowsException.translateToIOException(Unknown Source)
at sun.nio.fs.WindowsException.rethrowAsIOException(Unknown Source)
at sun.nio.fs.WindowsException.rethrowAsIOException(Unknown Source)
at sun.nio.fs.WindowsFileSystemProvider.implDelete(Unknown Source)
at sun.nio.fs.AbstractFileSystemProvider.deleteIfExists(Unknown Source)
at java.nio.file.Files.deleteIfExists(Unknown Source)
at jenkins.util.io.PathRemover.removeOrMakeRemovableThenRemove(PathRemover.java:237)
Also:   java.nio.file.FileSystemException: C:\Program Files (x86)\Jenkins\plugins\ruby-runtime\WEB-INF\lib\classes.jar: 另一个程序正在使用此文件，进程无法访问。

at sun.nio.fs.WindowsException.translateToIOException(Unknown Source)
at sun.nio.fs.WindowsException.rethrowAsIOException(Unknown Source)
at sun.nio.fs.WindowsException.rethrowAsIOException(Unknown Source)
at sun.nio.fs.WindowsFileSystemProvider.implDelete(Unknown Source)
at sun.nio.fs.AbstractFileSystemProvider.deleteIfExists(Unknown Source)
at java.nio.file.Files.deleteIfExists(Unknown Source)
at jenkins.util.io.PathRemover.removeOrMakeRemovableThenRemove(PathRemover.java:241)

### 分析

无

### 解决方案

将 Jenkins 移动到无空格的安装目录

#### 步骤

1. 停止 Jenkins 服务
1. 拷贝 Jenkins 整个文件夹到新的目录，比如 C:\Jenkins
1. 修改注册表项 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Jenkins\ImagePath 为新的路径
1. 启动 Jenkins 服务

## 参考

- <https://github.com/elvanja/jenkins-gitlab-hook-plugin/issues/19>
