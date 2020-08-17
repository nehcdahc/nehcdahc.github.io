---
layout: post
title: "Jenkins 常见问题"
date: 2020-08-17 14:00:00 +0800
categories: ["软件工程"]
tags: [Jenkins, CICD]
---

## 由于网络原因，Jenkins 插件可能无法正常安装

解决方案：

1. 尝试修改更新站点为可用的镜像站点，参考 [Jenkins 镜像状态](http://mirrors.jenkins-ci.org/status.html)

    打开 Jenkins > Manage Jenkins > Manage Plugins > Advanced，将 Update Site 的值由 <http://updates.jenkins-ci.org/update-center.json> 修改为 <https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json>

1. 手动安装插件

    1. 访问 <https://mirrors.tuna.tsinghua.edu.cn/jenkins/plugins/>，下载需要的插件
    1. 打开 Jenkins > Manage Jenkins > Manage Plugins > Advanced > Upload Plugin，进行插件上传
    1. 访问 <http://xxx.xxx.xxx.xxx:8080/restart>，以重启 Jenkins

## Jenkins 访问特别慢，且不消耗服务器资源

Jenkins 访问特别慢。CPU、内存、磁盘资源占用特别低。怀疑可能是配置问题。

解决方案：

1. 检查 jre 版本是否是 64 位的，如果不是，参考 <https://stackoverflow.com/questions/49457923/jenkins-jre-update> 进行调整。
1. 调整 C:\Program Files (x86)\Jenkins\Jenkins.xml 中内存参数

```xml
<service>
<!--> ... <!-->
<arguments>-Xrs -Xmx5120m -Dhudson.lifecycle=hudson.lifecycle.WindowsServiceLifecycle -jar "%BASE%\jenkins.war" --httpPort=8080 --webroot="%BASE%\war"</arguments>
<!--> ... <!-->
</service>
```

### 参考

- <https://stackoverflow.com/questions/49457923/jenkins-jre-update>
- <https://wiki.jenkins.io/display/JENKINS/Builds+failing+with+OutOfMemoryErrors>

## Jenkins 安装 ruby-runtime 出错

```
C:\Program Files (x86)\Jenkins\plugins\ruby-runtime\WEB-INF\lib\classes.jar: The process cannot access the file because it is being used by another process.

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
```

解决方案：

将 Jenkins 移动到无空格的安装目录

1. 停止 Jenkins 服务
1. 拷贝 Jenkins 整个文件夹到新的目录，比如 C:\Jenkins
1. 修改注册表项 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Jenkins\ImagePath 为新的路径
1. 启动 Jenkins 服务

### 参考

- <https://github.com/elvanja/jenkins-gitlab-hook-plugin/issues/19>

### Git Clone 失败

```
fatal: The remote end hung up unexpectedly
```

解决方案：

```
git config --global http.postBuffer 1048576000
Additional Behaviours: Advanced clone behaviours
timeout (in minutes) for clone and fetch operations: 30
```

## 参考

### Jenkins 镜像

> [Jenkins 镜像状态](http://mirrors.jenkins-ci.org/status.html)

- China
  - [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/jenkins/)
- Japan
  - <http://ftp.yz.yamagata-u.ac.jp/pub/misc/jenkins/>
  - <http://mirror.esuni.jp/jenkins/>
