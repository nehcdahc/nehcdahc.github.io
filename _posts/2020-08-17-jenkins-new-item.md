---
layout: post
title: "Jenkins 任务"
date: 2020-08-17 14:00:00 +0800
categories: ["软件工程"]
tags: [Jenkins, CICD]
---

## 创建基本任务

### General

- Use custom workspace > Directory: {directory, d:\github\}

### Source Code Management

#### Git plugin

- Git > Repositories > Repository URL: {repository_url, <https://xxx.xxx.xxx.xxx/xxx.git>}
- Git > Repositories > Credentials: {credentials}
- Git > Branches to build > Branch Specifier (blank for 'any'): {branch_specifier, 10.29}
- Git > Additional Behaviours > Check out to specific local branch > Branch name: {branch_name, 10.29}

### Build Triggers

- Build when a change is pushed to GitLab. GitLab webhook URL: ... > Enabled GitLab triggers > Push Events: {checked}
- Build when a change is pushed to GitLab. GitLab webhook URL: ... > Enabled GitLab triggers > Accepted Merge Request Events: {checked}
- Build when a change is pushed to GitLab. GitLab webhook URL: ... > Enabled GitLab triggers > Approved Merge Requests (EE-only): {checked}
- Build when a change is pushed to GitLab. GitLab webhook URL: ... > Secret token > {secret_token，如果这个不设置 GitLab 的 Webhooks 配置会报 403 错误}

#### 参考

- <https://github.com/jenkinsci/gitlab-plugin/issues/375>
- <https://www.cnblogs.com/kaerxifa/p/11671961.html>

### Build Environment

#### Version Number Plug-In

- Create a formatted version number > Environment Variable Name: BUILD_VERSION
- Create a formatted version number > Environment Variable Name: Version Number Format String: {branch_name, 10.29}.${BUILD_DATE_FORMATTED, "yyyyMMdd"}.${BUILDS_TODAY}
- Create a formatted version number > Inject environment variables to the build process: {checked}

##### 参考

- <https://www.cnblogs.com/wdliu/p/8312735.html>

#### NodeJS Plugin

- Provide Node & npm bin/ folder to PATH > NodeJS Installation: NodeJS12
- Provide Node & npm bin/ folder to PATH > npmrc file: - use system default -
- Provide Node & npm bin/ folder to PATH > Cache location: Default(~/.npm or %APP_DATA%\npm-cache)

### Build

#### PowerShell plugin

- PowerShell
  - Command

  ```powershell
  $env:Path+=";C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin"
  {build_script, .\build.ps1 -r}
  ```

### Post-build Actions

#### Artifact Deployer Plug-in

- Post-build Actions > [ArtifactDeployer] - Deploy the artifacts from build workspace to remote locations > Artifacts to deploy: {artifacts_to_deploy, \*\*}
- Post-build Actions > [ArtifactDeployer] - Deploy the artifacts from build workspace to remote locations > Basedir: {dist_directory, dist/}
- Post-build Actions > [ArtifactDeployer] - Deploy the artifacts from build workspace to remote locations > Remote File Location: {remote_file_location, f:/artifacts/\${BUILD_VERSION}}

> 企业微信通知，需要安装插件 [qy-wechat-notification](https://mirrors.tuna.tsinghua.edu.cn/jenkins/plugins/qy-wechat-notification/) 才能使用

- Webhook地址：<https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=${key}>
- 仅失败才@：✔
- 发送开始构建信息：✔
- 仅失败才发送：✔
- 仅成功才发送：✔
- 仅构建中断才发送：✔
- 仅不稳定构建才发送：✔
- 通知UserID: @${wechat_user_name}

## 基于模板创建任务

## 修改记录

- 2020-08-12 新增了企业微信通知的配置说明
