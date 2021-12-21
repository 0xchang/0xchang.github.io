---
title: centos7安装apache
date: 2021-11-24 17:08:42
tags: 
  - apache
  - centos7
---

 ## centos7 安装Apache

### 实验准备：vmware上安装的centos7

```shell
[root@cn7]/# uname -a
Linux cn7 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
[root@cn7]/# cat /etc/redhat-release
CentOS Linux release 7.9.2009 (Core)
```

### 安装Apache：

```shell
[root@cn7]/# yum -y install httpd
```

### 查看http状态：

```shell
[root@cn7]/# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: man:httpd(8)
           man:apachectl(8)
```

### 启动Apache并设置为开机启动：

```shell
[root@cn7]/# systemctl start httpd
[root@cn7]/# systemctl enable httpd
Created symlink from /etc/systemd/system/multi-user.target.wants/httpd.service to /usr/lib/systemd/system/httpd.service.
```

### 再次查看Apache状态：

```shell
[root@cn7]/# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since 六 2021-08-28 21:28:05 CST; 1min 17s ago
     Docs: man:httpd(8)
           man:apachectl(8)
 Main PID: 1358 (httpd)
   Status: "Total requests: 0; Current requests/sec: 0; Current traffic:   0 B/sec"
   CGroup: /system.slice/httpd.service
           ├─1358 /usr/sbin/httpd -DFOREGROUND
           ├─1359 /usr/sbin/httpd -DFOREGROUND
           ├─1360 /usr/sbin/httpd -DFOREGROUND
           ├─1361 /usr/sbin/httpd -DFOREGROUND
           ├─1362 /usr/sbin/httpd -DFOREGROUND
           └─1363 /usr/sbin/httpd -DFOREGROUND

8月 28 21:27:34 cn7 systemd[1]: Starting The Apache HTTP Server...
8月 28 21:27:54 cn7 httpd[1358]: AH00558: httpd: Could not reliably determine the server's fully qualified dom...essage
8月 28 21:28:05 cn7 systemd[1]: Started The Apache HTTP Server.
Hint: Some lines were ellipsized, use -l to show in full.
```

### 查看Apache版本

```shell
[root@cn7]/# httpd -v
Server version: Apache/2.4.6 (CentOS)
Server built:   Nov 16 2020 16:18:20
```

### 使用ss查看端口监听状态

（ss -tnl     [t：tcp,n：数字显示,l：监听]）

```shell
[root@cn7]/# ss -tnl
State      Recv-Q Send-Q               Local Address:Port                              Peer Address:Port
LISTEN     0      128                              *:22                                           *:*
LISTEN     0      100                      127.0.0.1:25                                           *:*
LISTEN     0      128                           [::]:22                                        [::]:*
LISTEN     0      100                          [::1]:25                                        [::]:*
LISTEN     0      128                           [::]:80                                        [::]:*
```

### 访问虚拟机的ip地址

如果能访问即成功。

