---
layout: post
title: "如何手动同步 Chrome 浏览器的数据"
date: 2020-04-14 11:04:00 +0800
categories: ["工具"]
---

常常遇到 Chrome 没有及时同步数据的问题，解决方案很简单：

## 方案一

1. Chrome 浏览器地址栏输入 chrome://sync-internals/ 或者 chrome://sync/
1. 依次点击 About > Stop Sync(Keep Data)，About > Request Start

## 方案二

1. make a change is to bookmark a new page and delete the bookmark. This will force chrome to sync the bookmarks.

## 参考

- <https://superuser.com/questions/129410/force-chrome-to-sync-bookmarks>
