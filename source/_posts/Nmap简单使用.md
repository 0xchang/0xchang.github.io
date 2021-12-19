---
title: Nmap简单使用
date: 2021-11-24 17:33:14
tags: nmap
---

## Nmap简单使用

### 躲避防火墙，分组发送报文

```shell
nmap -f -v 127.0.0.1
```

### 设定最大传输单元，必须是8的倍数

```shell
nmap --mtu 16 127.0.0.1
```

### 指定多个诱饵

```shell
nmap -D 192.168.61.130 ME 127.0.0.1  #(ME 后面接目标地址)
```

### 源地址欺骗

```shell
namp -sI www.baidu.com:80 127.0.0.1
```

### 源端口欺骗

```shell
nmap --source-port 88 127.0.0.1
```

### 根据发包长度（tcp是40字节，icmp溢出是28字节）

```shell
nmap --data-length 30 127.0.0.1
```

### MAC地址欺骗

```shell
nmap -sT -PN --spoof-mac 0 127.0.0.1
```

