---
title: arch开机启动脚本
date: 2022-02-10 21:35:42
tags:
- arch
- 开机启动
---

## Arch Linux 添加开机启动脚本

### 0x01 创建一个启动service脚本

```shell
sudo nano /usr/lib/systemd/system/rc-local.service
```

以下是rc-local.service的内容

```
[Unit]
Description="/etc/rc.local Compatibility"
[Service]
Type=forking
ExecStart=/etc/rc.local start
TimeoutSec=0
StandardInput=tty
RemainAfterExit=yes
SysVStartPriority=99
[Install]
WantedBy=multi-user.target
```

### 0x02 创建 /etc/rc.local 文件

```shell
sudo nano /etc/rc.local
```

以下是/etc/rc.local的内容

```shell
#!/bin/sh
# /etc/rc.local
if test -d /etc/rc.local.d; then
for rcscript in /etc/rc.local.d/*.sh; do
test -r "${rcscript}" && sh ${rcscript}
done
unset rcscript
fi
```

### 0x03 添加文件执行权限，添加/etc/rc.local.d文件夹，设置开机自启

```shell
sudo chmod a+x /etc/rc.local
sudo mkdir /etc/rc.local.d
sudo systemctl enable rc-local
```

### 0x04 创建sh脚本放在/etc/rc.local.d/文件夹中

```shell
sudo nano /etc/rc.local.d/paru.sh
(编写内容xxxx)
sudo chmod a+x /etc/rc.local.d/paru.sh
```

