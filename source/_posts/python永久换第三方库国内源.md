---
title: python永久换第三方库国内源
date: 2021-11-24 17:36:59
tags:
    - python
    - pip
---

## python永久换第三方库国内源

```powershell
#北京外国语大学镜像站
pip config set global.index-url https://mirrors.bfsu.edu.cn/pypi/web/simple
```

 国内靠谱的 pip 镜像源有：

(1)清华: **pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple**

(2)豆瓣: **pip config set global.index-url http://pypi.douban.com/simple/**

(3)阿里: **pip config set global.index-url http://mirrors.aliyun.com/pypi/simple/**

(4)腾讯: **pip config set global.index-url https://mirrors.cloud.tencent.com/pypi/simple/**

