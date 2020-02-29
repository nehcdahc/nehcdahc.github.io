---
layout: post
title: "nvm-windows"
date: 2020-02-29 14:03:00 +0800
categories: ["工具"]
---

nvm-windows 是一个 Windows 的 node.js 版本管理工具。

- [点击这里下载](https://github.com/coreybutler/nvm-windows/releases)。
- 必须使用管理员身份运行 powershell 或者其他 Windows 终端程序

## 常用命令

```powershell
# 设定 node 镜像，建议中国用户使用 https://npm.taobao.org/mirrors/node/
nvm node_mirror {node_mirror_url}

# 设定 npm 镜像，建议中国用户使用 https://npm.taobao.org/mirrors/npm/
nvm npm_mirror {npm_mirror_url}

# 查看已经安装的 node.js 版本
nvm list

# 查看可以安装的 node.js 版本
nvm list availabe

# 切换 node.js 版本
nvm use {node_version}
```
