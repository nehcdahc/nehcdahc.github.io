---
layout: post
title: "我的 mRemoteNG 使用规则"
date: 2020-03-06 19:12:00 +0800
categories: ["工具"]
---

[mRemoteNG - Multi-Remote Next Generation](https://mremoteng.org/) is a fork of mRemote: an open source, tabbed, multi-protocol, remote connections manager. mRemoteNG adds bug fixes and new features to mRemote.

## 我的使用规则

1. 目录划分为 Development、Test、Project
1. 链接命名规则
   1. 使用“地理位置 IP”的方式，示例：azure 40.73.78.211、office 192.168.6.200
   1. 使用“所有组织 机器用途（test、dev、prod，其中 prod 为默认值） 项目或产品名称 服务器类型属性（可选，app、db、postgres） 地理位置 IP”，示例：{org_name} www azure 139.217.235.117
1. 备份到 OD\Temp 目录，命名为 mremote.from.office.to.home.202002251059.xml
