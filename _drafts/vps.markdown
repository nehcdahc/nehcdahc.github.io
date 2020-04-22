---
layout: post
title: VPS
date: 2020-01-19 00:00:00 +8000
categories:
tags: [SSR, VPS]
---

## SSR

1. 执行以下命令进行安装

   ```bash
   wget --no-check-certificate https://freed.ga/github/shadowsocksR.sh; bash shadowsocksR.sh
   ```

1. 如果需要加速，可以安装 Google BBR，参考这篇 <https://laod.cn/black-technology/centos7-google-bbr-vps.html> 文章

### 注意

1. 推荐使用 CentOS 6。如果使用 CentOS 7+ 需要关闭防火墙

   ```bash
   systemctl stop firewalld.service
   systemctl disable firewalld.service
   ```

1. 参考 [小文's blog](www.qcgzwx.cn)
1. [相关资源](https://freed.ga)
