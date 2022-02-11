---
title: metasploit溢出UnrealIRCd后门漏洞
date: 2021-12-21 12:23:58
tags: 
    - metasploit
    - unrealircd
    - 后门
    - 漏洞
---

## metasploit溢出UnrealIRCd后门漏洞

### 目的：利用UnrealIRCd后门漏洞，获取目标主机的root权限

### 原理：某些站点的UnrealIRCd，在DEBUG3_DOLOG_SYSTEM宏中包含外部引入的恶意代码，远程攻击者能够执行任意代码。

### 过程：

利用nmap扫描主机，确定`6667`端口开放，对应服务为`unreal ircd`

![1](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E6%BA%A2%E5%87%BAUnrealIRCd%E5%90%8E%E9%97%A8%E6%BC%8F%E6%B4%9E/2.PNG)

终端输入msfconsole，进入msf后输入`search unreal ircd`，搜索相关工具和攻击载荷

![2](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E6%BA%A2%E5%87%BAUnrealIRCd%E5%90%8E%E9%97%A8%E6%BC%8F%E6%B4%9E/2.PNG)

终端输入命令`use exploit/unix/irc/unreal_ircd_3281backdoor`,启用漏洞模块

输入show options,查看需要设置的相关选项
![3](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E6%BA%A2%E5%87%BAUnrealIRCd%E5%90%8E%E9%97%A8%E6%BC%8F%E6%B4%9E/3.PNG)
设置目标ip

run或者exploit,开始攻击，攻击成功后，建立shell会话

![4](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E6%BA%A2%E5%87%BAUnrealIRCd%E5%90%8E%E9%97%A8%E6%BC%8F%E6%B4%9E/4.PNG)

终端输入whoami，查看权限root，输入cat /etc/passwd，查看系统账户

![5](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E6%BA%A2%E5%87%BAUnrealIRCd%E5%90%8E%E9%97%A8%E6%BC%8F%E6%B4%9E/5.PNG)

本机4444端口建立远程会话
