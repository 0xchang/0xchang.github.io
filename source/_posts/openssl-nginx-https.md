---
title: openssl+nginx+https
date: 2022-09-26 10:03:08
tags:
- nginx
- openssl
- https
---

## 使用openssl和nginx配置88端口为https协议
* 该方法的证书属于无效证书,可以从第三方认证机构购买,原文:https://www.jb51.net/article/217964.htm

### 安装nginx（跳过）

### 创建CA证书
1. 生成CA私钥
`root@debian:~# openssl genrsa -out local.key 2048`
![CA私钥](https://121.5.125.62:88/image/openssl-nginx-https/CA私钥.JPG)
2. 生成CA证书请求
`root@debian:~# openssl req -new -key local.key -out local.csr`
![CA证书请求](https://121.5.125.62:88/image/openssl-nginx-https/CA证书请求.JPG)
3. 生成CA根证书
`openssl x509 -req -in local.csr -extensions v3_ca -signkey local.key -out local.crt`
![CA根证书](https://121.5.125.62:88/image/openssl-nginx-https/CA根证书.JPG)

### 配置nginx
```
server{
        listen 88 ssl;                              #监听88端口，并开启ssl
        keepalive_timeout 100;                      #开启keepalive 激活keepalive长连接,减少客户端请求次数
        ssl_certificate /root/local.crt;            #server端证书位置
        ssl_certificate_key /root/local.key;        #server端私钥位置
        ssl_session_cache shared:SSL:10m;           #缓存session会话
        ssl_session_timeout 10m;                    # session会话    10分钟过期
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;
        server_name localhost;
        charset utf-8;

        #默认使用302临时重定向,不改变请求的方法(如post还是post),301永久重定向,307临时重定向不改变请求的方法
        error_page 497 301 =307 https://$host:$server_port$request_uri;   

        #根目录
        root /var/www/test;

        location / {
          index index.html;
        }
}
```
