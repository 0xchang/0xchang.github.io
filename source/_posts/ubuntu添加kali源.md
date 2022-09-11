---
title: ubuntu添加kali源
date: 2022-09-11 16:08:53
tags:
---


### 原博客
https://www.cnblogs.com/liyilong/p/13789586.html

### 添加源
```
deb https://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src https://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
```

### 设置公钥
1. 方法一
```
apt-key adv --keyserver keyserver.ubuntu.com --recv ED444FF07D8D0BF6
```

2. 方法二
```
gpg --keyserver hkp://keyserver.ubuntu.com:11371 --recv-keys ED444FF07D8D0BF6
gpg --export --armor ED444FF07D8D0BF6 |apt-key add -
```
