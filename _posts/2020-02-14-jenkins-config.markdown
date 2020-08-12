---
layout: post
title: "常用 Jenkins 配置"
date: 2020-02-14 16:59:00 +0800
categories: ["软件工程"]
tags: [Jenkins, CICD]
---

## General

- Use custom workspace
  - Directory: {directory, d:\github\\}

## Source Code Management

### Git plugin

- Git
  - Repositories
    - Repository URL: {repository_url, <https://github.com/nehcdahc/nehcdahc.github.io.git>}
    - Credentials: {credentials}
  - Branches to build
    - Branch Specifier (blank for 'any'): {branch_specifier, 10.29}
  - Additional Behaviours
    - Check out to specific local branch
      - Branch name: {branch_name, 10.29}

## Build Triggers

- Build when a change is pushed to GitLab. GitLab webhook URL: ...
  - Secret token: 如果这个不设置 GitLab 的 Webhooks 配置会报 403 错误

### 参考

- <https://github.com/jenkinsci/gitlab-plugin/issues/375>
- <https://www.cnblogs.com/kaerxifa/p/11671961.html>

## Build Environment

### Version Number Plug-In

- Create a formatted version number
  - Environment Variable Name: BUILD_VERSION
  - Version Number Format String: {branch_name, 10.29}.${BUILD_DATE_FORMATTED, "yyyyMMdd"}.${BUILDS_TODAY}
  - Inject environment variables to the build process: Checked

#### 参考

- <https://www.cnblogs.com/wdliu/p/8312735.html>

### NodeJS Plugin

- Provide Node & npm bin/ folder to PATH
  - NodeJS Installation:
  - npmrc file:
  - Cache location:

## Build

### PowerShell plugin

- PowerShell
  - Command

  ```powershell
  $env:Path+=";C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin"
  {build_script, .\build.ps1}
  ```

## Post-build Actions

### Artifact Deployer Plug-in

- Post-build Actions
  - [ArtifactDeployer] - Deploy the artifacts from build workspace to remote locations
    - Artifacts to deploy: {artifacts_to_deploy, \*\*}
    - Basedir: {dist_directory, dist/}
    - Remote File Location: {remote_file_location, f:/artifacts/\${BUILD_VERSION}}

- 企业微信通知

  > 需要安装插件 [qy-wechat-notification](https://mirrors.tuna.tsinghua.edu.cn/jenkins/plugins/qy-wechat-notification/) 才能使用

  - Webhook地址: <https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=${key}>
  - 发送开始构建信息: ✔
  - 通知UserID: @${wechat_user_name}

## 修改记录

- 2020-08-12 新增了企业微信通知的配置说明
