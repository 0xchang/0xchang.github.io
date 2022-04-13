---
title: ssh文件权限问题
date: 2022-03-06 20:07:17
tags:
 -ssh
 -权限
 -远程ssh连接
---

## ssh文件权限问题
### 0x00 权限过高
今天使用mobaxterm生成的公钥的上传到服务器，准备登陆时反馈信息，server refused our key，主要原因是~/.ssh文件夹权限过高导致的问题，authorized_keys文件我给的权限是644，.ssh文件夹我给的权限是755，最后使用密钥登录成功。
