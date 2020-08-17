---
layout: post
title: "Jenkins 配置"
date: 2020-08-17 14:00:00 +0800
categories: ["软件工程"]
tags: [Jenkins, CICD]
---

## Manage Jenkins

> 配置并行任务数

- System Configuration > Configure System > Maven Project Configuration > # of executors: {#\_of_executors, 3}

> 配置 Jenkins 访问地址

- System Configuration > Configure System > Maven Project Configuration > Jenkins Location > Jenkins URL: {jenkins_url, <http://192.168.1.1:8080/>}

> 配置语言，该选项依赖插件 locale 和 localization-zh-cn

- System Configuration > Configure System > Locale >　 Default Language: {default_language, zh_cn}

## Global Tool Configuration

> MSBuild 配置

- MSBuild > MSBuild installations > MSBuild > Name: {msbuild_name, MSBuild2019}
- MSBuild > MSBuild installations > MSBuild > Path to MSBuild: {path_to_msbuild, C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\}

> Node.js 配置

- NodeJS > NodeJS installations > NodeJS > Name: {NodeJS12}
- NodeJS > NodeJS installations > NodeJS > Install automatically: {checked}
- NodeJS > NodeJS installations > NodeJS > Install from nodejs.org > Version: {NodeJS 12.18.3}
- NodeJS > NodeJS installations > NodeJS > Install from nodejs.org > Force 32bit architecture: {unchecked}
- NodeJS > NodeJS installations > NodeJS > Install from nodejs.org > Global npm packages to install: {yarn@1.22.4 nrm@1.1.0 npm-check-updates@4.1.2 node-sass@4.14.1 --registry=<http://xxx.xxx.xxx.xxx:8080/repository/npm-all/>}
- NodeJS > NodeJS installations > NodeJS > Install from nodejs.org > Global npm packages refresh hours: {168}

## Config File Management

- Npm config file > NPM Registry > URL: <http://xxx.xxx.xxx.xxx/repository/npm-hosted/>
- Npm config file > NPM Registry > Credentials: - none -
- Npm config file > Content

```
registry=http://xxx.xxx.xxx.xxx/repository/npm-all/
//xxx.xxx.xxx.xxx/repository/npm-hosted/:_authToken=NpmToken.xxxxxx-xxxx-xxxx-xxxx-xxxxxxxx
```
