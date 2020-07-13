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
1. 添加函数应用

    - 函数应用名称：wechatwork-robot
    - 运行时堆栈：Node.js

1. 打开函数应用 wechatwork-robot > 开发工具 > 应用服务编辑器（预览版） —— 好像是 VSCode 为 Azure 做的云版本编辑器
    - 因为我们要使用 axios 组件，所以要在 package.json 文件中添加依赖的包 axios。如果没有发现 package.json 文件，需要额外创建 package.json 文件。

    ```json
    {
        "name": "xxx-gitlab-hook",
        "dependencies": {
            "axios": "^0.19.2"
        }
    }
    ```

    - 创建 wechatwork.js 文件，这里实现了 Markdown 格式的消息发送到企业微信，其他格式可以参考企业微信文档实现

    ```javascript
    const axios = require('axios');
    const wechatWebHookUrl = "https://qyapi.weixin.qq.com/cgi-bin/webhook/";

    // https://work.weixin.qq.com/help?doc_id=13376
    // 每个机器人发送的消息不能超过20条/分钟。
    class wechatwork {
        async sendMarkdownMessage(robotId, content, options) {
            const markdownMessageInfo = {
                "msgtype": "markdown",
                "markdown": {
                    "content": content
                }
            };

            return await axios.post(`${wechatWebHookUrl}send?key=${robotId}`, markdownMessageInfo)
                .then((res) => {
                    console.log(`Status: ${res.status}`);
                    console.log('Body: ', res.data);
                }).catch((err) => {
                    console.error(err);
                });
        }
    }

    module.exports = wechatwork;
    ```

1. 在函数应用 wechatwork-robot 中添加各种消息源解析函数，以 GitLab 为例
    - 新建函数
        - 选择 HTTP trigger
            - 新建函数：GitLab-Hook —— 函数的名称
            - Authorization level：Anonymous —— 访问权限，Azure 提供了 Function，Anonymous 和 Admin
    - 编辑函数 GitLab-Hook
        - 代码 + 测试
            - 修改 wechatwork-robot\GitLab-Hook\index.js 文件，注意其中 wechatwork.js 是单独的文件，后面会讲如何撰写这个文件

            ```javascript
            const wechatwork = require('../wechatwork.js');

            function handleDefaultEvent(event) {
                return new Array(`Git: Sorry, event ${event} is not supported.`);
            }

            function addZentaoLink(commitMessage) {
                // ...
                // 我们使用了禅道管理迭代 Task 和 Bug，所以对 GitLab 的消息做了些适配处理
                // ...

                return newCommitMessage;
            }

            function handlePush(context) {
                let {user_name, project:{name:proName, web_url}, commits} = context;

                return commits.map(commit => {
                    let commitMessage = addZentaoLink(commit.message);
                    const message = `Git：项目 [${proName}](${web_url}) 接收到来自 ${user_name} 的推送：[${commit.id.substr(0,8)}](${web_url}/-/commit/${commit.id}) ${commitMessage} `;
                    return message;
                });
            }

            function handleMergeRequest(context) {
                let {object_kind='', user:{name, avatar_url}, project:{name:proName, web_url}, object_attributes:{title, state, target_branch, source_branch, url}} = context;
                const message = `Git：项目 [${proName}](${web_url}) 由 @${name} ${state} 一个 ${object_kind}：[${title}](${url})。源分支：${source_branch}，目标分支：${target_branch}`;
                return new Array(message);
            }

            function handleNoteRequest(context) {
                let {
                    object_kind='',
                    user:{name},
                    project:{name:proName, web_url},
                    object_attributes:{note, noteable_type, url}
                } = context;
                const message = `Git：项目 [${proName}](${web_url}) 接收到来自 @${name} 的对 ${noteable_type} 的[评论](${url})：${note}`;
                return new Array(message);
            }

            module.exports = async function (context, req) {
                const gitEvent = req.headers["x-gitlab-event"];
                const rebotId = req.query.id;

                let messages = [];
                switch(gitEvent) {
                    case "Push Hook":
                        messages = handlePush(req.body);
                        break;
                    case "Merge Request Hook":
                        messages = handleMergeRequest(req.body);
                        break;
                    case "Note Hook":
                        messages = handleNoteRequest(req.body);
                        break;
                    default:
                        messages = handleDefaultEvent(gitEvent);
                        break;
                }

                let work = new wechatwork();
                messages.forEach(message => {
                    work.sendMarkdownMessage(rebotId, message);
                });

                context.res = {
                    body: "Done"
                };
            }
            ```

        - Test/Run（可选）
          在上述代码完成后，可以使用内置的测试运行功能可以模拟 GitLab 的 Http 请求进行测试

    - 获取函数 URL，以供后续 GitLab Webhooks 配置使用

### 添加 GitLab WebHooks

1. 登录 GitLab
1. 定位到 Settings > Webhooks，填写后 Add Webhook
    1. URL: 上面获取到的函数 URL
    1. Trigger
        1. Push events
        1. Comments
1. 添加成功后，可以在下方的 Project Hooks 点击 Test，查看企业微信群是否能收到消息推送

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
