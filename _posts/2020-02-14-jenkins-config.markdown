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
