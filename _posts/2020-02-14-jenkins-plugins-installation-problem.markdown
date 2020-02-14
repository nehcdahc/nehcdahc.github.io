---
layout: post
title: "Jenkins 插件安装问题"
date: 2020-02-14 10:52:00 +0800
categories: ["软件工程"]
tags: [Jenkins, CICD]
---

## 插件安装问题

1. 尝试修改更新站点为可用的镜像站点

   打开 Jenkins > Manage Jenkins > Manage Plugins > Advanced，将 Update Site 的值由 <http://updates.jenkins-ci.org/update-center.json> 修改为 <https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json>

1. 手动安装插件

   1. 访问 <https://mirrors.tuna.tsinghua.edu.cn/jenkins/plugins/>，下载需要的插件
   1. 打开 Jenkins > Manage Jenkins > Manage Plugins > Advanced > Upload Plugin，进行插件上传
   1. 访问 <http://localhost:8080/restart>，以重启 Jenkins

## Jenkins 镜像

> [Jenkins 镜像状态](http://mirrors.jenkins-ci.org/status.html)

- China
  - [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/jenkins/)
- Japan
  - <http://ftp.yz.yamagata-u.ac.jp/pub/misc/jenkins/>
  - <http://mirror.esuni.jp/jenkins/>
