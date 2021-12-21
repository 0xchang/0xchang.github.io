---
title: metasploit之PostgreSQL
date: 2021-12-21 12:24:10
tags: msf PostgreSQL
---

## metasploit之PostgreSQL

### 目的： Metasploit自带数据库（PostgreSQL）操作

### 原理： 
`db_status`     查看数据库连接状态

`db_connect`  链接数据库

`db_disconnect`    断开连接

`vulns`     查看数据库扫描的主机的漏洞

`workspace`   工作空间，相对于独立的

### 过程：

打开终端，运行msfconsole,`db_status`查看数据库连接状态，如果已连接执行`db_disconnect`

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/1.PNG)

exit退出软件，执行`service postgresql start`,启动数据库，`netstat -ntlp | grep postgre`查询是否启动成功，端口为`5432`

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/2.PNG)

重新运行msf,`db_status`查看是否连接`数据库`

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/3.PNG)

`db_disconnect`断开连接，`db_status`查看连接状态

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/4.PNG)

手动使用命令连接数据库，须知`数据库名`，`用户名`，`ip`以及`密码`，新建窗口，查询数据库配置文件，执行`find / -name database.yml`

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/5.PNG)

`cat /usr/share/metasploit-framework/config/database.yml`，查看配置文件

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/6.PNG)

返回msf界面，db_connect查看使用方法，选择第二种`db_connect 用户名:密码@ip/数据库名`，配置信息是`development`标签内的

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/7.PNG)
![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/8.PNG)

退出msf，执行`su postgres`

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/9.PNG)

创建新用户sub1及密码：`createuser sub1 -P`,回车并输入密码123456两次，`createdb --owner=sub1 test`连接数据库test，完成后exit退出

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/10.PNG)

运行msfconsole，db_disconnect断开连接，执行`db_connect sub1:123456@localhost/test`连接刚才的数据库，`db_status`查看状态

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/11.PNG)

退出msf,编辑`/etc/postgresql/9.5/main/postgresql.conf`文件，`shift+：`输入`set number`调出行号

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/12.PNG)

找到59行，将#去掉，并且把localhost改为*，88行去掉#,然后保存退出

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/13.PNG)

编辑/etc/postgresql/9.5/main/pg_hba.conf,最后一行添加host all all 0.0.0.0/32 md5，保存退出

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/14.PNG)

service postgresql restart，重启服务

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/15.PNG)

打开msf，workspace --help查看用法

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/16.PNG)

workspace -v查看工作空间，只有默认的一个

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/17.PNG)

worksapce -a 1,workspace -a 2 添加工作空间1，2，执行workspace -v 查看工作空间

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/18.PNG)

workspace -d 1删除工作空间1，执行workspace -v 查看现有工作空间

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/19.PNG)

workspace 2切换到工作空间2

workspace -d 2删除工作空间2,自动切换到默认工作空间

![](https://gitee.com/oxchang/img-host/raw/master/metasploit%E4%B9%8BPostgreSQL/20.PNG)