---
layout: post
title: "Jenkins 访问特别慢，且不消耗服务器资源"
date: 2020-07-14 17:38:00 +0800
categories: ["软件工程"]
tags: [Jenkins, CICD]
---

## 现象

Jenkins 访问特别慢

## 分析

CPU、内存、磁盘资源占用特别低。怀疑可能是配置问题。

## 方案

1. 检查 jre 版本是否是 64 位的，如果不是，参考 <https://stackoverflow.com/questions/49457923/jenkins-jre-update> 进行调整。
1. 调整 C:\Program Files (x86)\Jenkins\Jenkins.xml 中内存参数

```xml
<service>
<!--> ... <!-->
<arguments>-Xrs -Xmx5120m -Dhudson.lifecycle=hudson.lifecycle.WindowsServiceLifecycle -jar "%BASE%\jenkins.war" --httpPort=8080 --webroot="%BASE%\war"</arguments>
<!--> ... <!-->
</service>
```

## 参考

- <https://stackoverflow.com/questions/49457923/jenkins-jre-update>
- <https://wiki.jenkins.io/display/JENKINS/Builds+failing+with+OutOfMemoryErrors>
