---
title: rust修改默认环境变量与源
date: 2022-02-11 16:27:31
tags:
- rust
- 环境变量
---

## rust修改默认环境变量与源
### 0x00 windows rust
当使用windows安装rust时，默认安装位置是C:\User\xxx\\.cargo和C:\User\xxx\\.rustup

可以将目录粘贴到指定的文件夹位置，然后配置参数即可

打开 ***cmd*** 配置参数

```cmd
setx CARGO_HOME "D:\Program Files\.cargo"
setx RUSTUP_HOME "D:\Program Files\.rustup"
setx RUSTUP_UPDATE_ROOT http://mirrors.ustc.edu.cn/rust-static/rustup
setx RUSTUP_DIST_SERVER http://mirrors.ustc.edu.cn/rust-static
```

path 添加%CARGO_HOME%\bin

### 0x01 Linux rust 换源

在~/.cargo目录下新建config文件，然后输入以下内容

```config
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = 'ustc'
[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
```

