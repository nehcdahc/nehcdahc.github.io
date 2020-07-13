---
layout: post
title: "使用 Azure 函数应用推送消息到企业微信机器人"
date: 2020-06-23 16:35:00 +0800
tags: ["WeChat Work", "Robot", "GitLab", "Azure", "语雀"]
categories: ["解决方案", "虚拟化平台"]
---

## 基本原理

我们有时会希望工作时使用同一个工具接管所有系统的消息。

比如使用企业微信接管 GitLab、语雀、禅道等系统的 Webhooks。同时由于消息源（GitLab、禅道、……）系统输出的消息格式和监管工具接收的格式可能不一致，所以我们通常需要做消息格式转换。那么转换的工具可以自己构建一个应用发布出去，或者直接使用云服务（腾讯云函数、Azure 函数应用、……）构建。

本文结合 GitLab Webhooks，Azure 函数和企业微信的群机器人实现上述过程。

其中：

1. GitLab 为消息源，也可以是禅道、语雀、Gitee 或者自己构建的任何应用
1. Azure 函数应用为消息转换转发工具，也可以是腾讯云函数等第三方云服务或者自己构建的任何应用
    - [ngrok](https://ngrok.com/)
1. 企业微信的群机器人为消息的接收方，也可以是钉钉、倍恰或者自己构建的任何应用

## 示例步骤

### 创建企业微信群机器人

具体可以参考[如何配置群机器人？-帮助中心-企业微信](https://work.weixin.qq.com/help?doc_id=13376)。

### 创建 Azure 函数应用

1. 登录 Azure 的 Portal
1. 创建函数应用

    - 函数应用名称：wechatwork-robot

### 添加 GitLab WebHooks

## 参考

- [如何配置群机器人？-帮助中心-企业微信](https://work.weixin.qq.com/help?doc_id=13376)
- [Webhooks · 语雀](https://www.yuque.com/yuque/developer/doc-webhook)
- <https://github.com/ydCao/gitLab-robot-workwechat>
- <https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook>
- <https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-node>
- <https://docs.microsoft.com/zh-cn/azure/azure-functions/functions-create-serverless-api>
- <https://docs.microsoft.com/zh-cn/azure/azure-functions/functions-reference-node#environment-variables>
- <https://docs.microsoft.com/zh-cn/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-javascript>
- <https://www.jianshu.com/p/d1d6574c259d>
- <https://github.com/ydCao/gitLab-robot-workwechat>
