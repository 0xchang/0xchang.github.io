---
title: linux修改用户名
date: 2023-03-03 10:56:27
tags:
- linux
- 用户名
- usermod
- groupmod
---

修改用户名需要注意要同时修改用户名，家目录，组用户，使用root权限修改，且要修改的用户不能登录系统。

### 修改用户
* 修改用户名，还需要修改用户家目录
* `usermod -l newname -d /home/newname -m oldname`
### 修改用户组
* `groupmod -n newgroup oldgroup`

