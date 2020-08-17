---
layout: post
title: "Jenkins 安装"
date: 2020-08-17 14:00:00 +0800
categories: ["软件工程"]
tags: [Jenkins, CICD]
---

## 前置条件

1. 操作系统要修改成中文区域，同时双字符要设置为中文
1. 时区要设置为东八区

## 安装 Jenkins

### 在 Windows 上安装 Jenkins

1. 从 [https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 下载安装 Java 8
2. 从 [https://jenkins.io/download/](https://jenkins.io/download/) 下载最新版本

### 在 Ubuntu 上安装 Jenkins

1. add the repository key to the system.

```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
```

1. append the Debian package repository address to the server's sources.list:

```bash
deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
```

1. When both of these are in place, we'll run update so that apt-get will use the new repository:

```bash
sudo apt-get update
```

1. Finally, we'll install Jenkins and its dependencies, including Java:

```bash
sudo apt-get install jenkins
```

## 参考

- [https://jenkins.io/doc/book/installing/](https://jenkins.io/doc/book/installing/)
