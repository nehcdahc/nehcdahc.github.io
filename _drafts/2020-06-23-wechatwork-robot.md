---
layout: post
title: "使用 Azure 函数应用推送 GitLab 消息到企业微信机器人"
date: 2020-06-23 16:35:00 +0800
tags: ["WeChat Work", "Robot", "GitLab", "Azure"]
categories: ["解决方案", "虚拟化平台"]
---

## 基本原理

使用 GitLab 的 Webhooks 推送事件到 Azure 函数应用，由 Azure 函数应用转换 GitLab 事件消息，然后转发给企业微信的群机器人。

其中：

1. GitLab 为消息源，也可以是禅道、语雀、Gitee 或者自己构建的任何应用
1. Azure 函数应用为消息转换转发工具，也可以是腾讯云函数等第三方云服务或者自己构建的任何应用
    - [ngrok](https://ngrok.com/)
1. 企业微信的群机器人为消息的接收方，也可以是钉钉、倍恰或者自己构建的任何应用

## 示例步骤

1. 创建一个企业微信群机器人。

注意：

- 必须是企业微信管理员才能创建群机器人

1. 创建 Azure 函数应用
1. 添加 GitLab WebHooks

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
