---
title: centos7安装mariadb
date: 2021-11-24 17:20:20
tags: centos7 mariadb
---

## centos7安装mariadb

mariadb是MYSQL的一个分支，语法基本和mysql一样，而且兼容mysql，centos7中自带了mariadb的软件包。

```shell
[root@cn7]/# cat /etc/redhat-release
CentOS Linux release 7.9.2009 (Core)
[root@cn7]/# uname -a
Linux cn7 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

### 安装mariadb：

```shell
[root@cn7]/# yum -y install mariadb-server
```

### 开启mariadb并设置开机启动：

```shell
[root@cn7]/# systemctl start mariadb
[root@cn7]/# systemctl enable mariadb
Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.
```

### 配置数据库

```shell
[root@cn7]/# mysql_secure_installation
Enter current password for root (enter for none):  # 输入数据库超级管理员root的密码(注意不是系统root的密码)，第一次进入还没有设置密码则直接回车

Set root password? [Y/n]  # 设置密码，y

New password:  # 新密码
Re-enter new password:  # 再次输入密码

Remove anonymous users? [Y/n]  # 移除匿名用户， y

Disallow root login remotely? [Y/n]  # 拒绝root远程登录，n，不管y/n，都会拒绝root远程登录

Remove test database and access to it? [Y/n]  # 删除test数据库，y：删除。n：不删除，数据库中会有一个test数据库，一般不需要

Reload privilege tables now? [Y/n]  # 重新加载权限表，y
```

### 启动数据库

```shell
[root@cn7]/# mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.5.68-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.00 sec)

MariaDB [(none)]> quit;
Bye
```

