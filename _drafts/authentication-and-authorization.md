---
layout: post
title: "身份认证与授权"
date: 2020-06-10 16:09:00 +0800
categories: ["解决方案"]
---

## 身份认证与授权

Authentication (AuthN) VS Authorization (AuthZ) <sup>[1]</sup>

|      | 身份认证                                                                                                                                      | 授权                                                                               |
| :--- | :-------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------- |
| 含义 | 验证用户是否与他们自己声明的身份一致                                                                                                          | 通过验证的用户，基于权限控制系统是否被允许该进行系统的访问、编辑、删除或者创建内容 |
| 依据 | 密码（Password）、双因素（2FA）、多因素（MFA）、数字证书（X509 Certificates）、生物识别（Biometric authenticators）、Web 身份验证（WebAuthn） | Access control for URI、Access control lists 等                                    |

## 身份认证方法

### 单因素

- 密码（Password）
- 安全密码（Security Pin）
- 美国联邦政府个人身份验证（Personal Identity Verification, PIV）
- ...

安全性较差，数据容易泄露

### 双因素

增加其他因素认证，例如

- 注册设备生成的令牌
- 一次性密码（One Time Passwords, OTP）
- 安全密码（Security Pin）
- ...

据赛门铁克研究，数据泄露的 80％ 可以通过 2FA 阻止

### 多因素

- 知道的密码（Password）或者安全密码（Security Pin）
- 拥有的手机或者安全令牌
- 指纹或 FaceID
- 输入的速度、位置信息
- ...

## 身份认证方法协议

如果我们在现有的服务（比如一个应用中）范围内访问另一个第三方的服务，通过共享身份验证方式（即采用相同的验证方式供第三方服务使用）是有安全风险的。因为第三方服务有可能会存储我们的凭据或者用户数据。
所以我们应该采用一些身份认证协议防止用户数据暴露给第三方或者攻击者。这些协议有：

- OAuth
- OpenID
- SAML
- FIDO
- ...

## 用户账户管理

用户账户也不一定是存放在数据库中的一张表，在一些企业 IT 系统中，对账户管理和权限有了更多的要求。所以账户技术 （accounting）可以帮助我们使用不同的方式管理用户账户，同时具有不同系统之间共享账户的能力。<sup>[3]<sup>

例如：Active Directory 是基于数据库的系统，可在 Windows 环境中提供身份验证，目录，策略和其他服务。LDAP（轻型目录访问协议）是一种应用协议，用于查询和修改目录服务提供者（如 Active Directory）中的项目，该服务支持 LDAP 形式。简单来说：AD 是目录服务数据库，而 LDAP 是用来与之交谈的协议之一。<sup>[2]</sup>

## 访问控制策略（AC）

如果我们需要把资源的权限划分到一个很细的粒度，就不得不考虑用户以何种身份来访问受限的资源

- 基于访问控制列表（ACL）
- 基于用户角色的访问控制（RBAC）
- ...

## HTTP 认证协议 <sup>[9]</sup>

- Bearer（OAuth）
- Basic（Http）
- Digest（Http）
- MAC

### Web 凭证技术 —— 管理身份认证结果

- Session（Cookie） Based —— 有状态，服务器上可以存储在数据库或者缓存
- Token Based —— 无状态
  - JWT
  - SWT
  - ...

## H2M M2M

## 参考

1. <https://docs.microsoft.com/zh-cn/azure/active-directory/develop/authentication-vs-authorization>
1. <https://stackoverflow.com/questions/663402/what-are-the-differences-between-ldap-and-active-directory#:~:text=active%20directory%20is%20the%20directory,that%20is%20ad%20or%20adam.&text=LDAP%20sits%20on%20top%20of,It%20is%20environment%20agnostic.>
1. <https://insights.thoughtworks.cn/api-2/>
1. <https://www.okta.com/blog/2019/02/the-ultimate-authentication-playbook/>
1. <https://zhuanlan.zhihu.com/p/36832100>
1. <https://obeta.me/posts/2019-03-01/WebAuthn%E4%BB%8B%E7%BB%8D%E4%B8%8E%E4%BD%BF%E7%94%A8>
1. <https://www.w3.org/2019/03/pressrelease-webauthn-rec.html.zh-hans>
1. <https://learnku.com/articles/22616>
1. <https://www.dazhuanlan.com/2019/10/16/5da5f5e65f7a6/?__cf_chl_jschl_tk__=23de1fac14c11fc48b02fc987ceea0d0a781ac31-1591789382-0-ARQPSgKcIYkFGH9q3lzTiEK6lDSGyvy_1cxzqGU9QwTx6II9BmZKzDPYlJ621ojRlWP2d6D22uEToFF5jf18MHKIS2pQb7irMQL9nHFw1eKpspeu7FjYhQ_RVpcSGFC1-a1hKG9ElOMllQNsMYnKsiDCDB51RNJGgDqKTjoKphAqRznWlaUGeA33zuMw13v0BgnM0PHJhdAY68d_bkvkm8hl8uUW7PdXroArhEuX8w52ZvHRE9jmzV4SGenS1Vm-RA8W3Z1DYNw6QIo2nSDKYzLtFdYD25QTd2TNmSmvXbuC3MA2ntWNJSmrpgMmHk7ogQ>
