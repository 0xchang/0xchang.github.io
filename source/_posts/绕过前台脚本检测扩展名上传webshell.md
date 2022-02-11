---
title: 绕过前台脚本检测扩展名上传webshell
date: 2021-12-27 09:56:35
tags: -webshell
    -前端
---

目的：
* 理解绕过前台脚本检测扩展名上传的原理 
* 学习绕过前台脚本检测扩展名上传的过程 

原理：
* 当用户在客户端选择文件点击上传的时候，客户端还没有向服务器发送任何消息，就对本地文件进行检测来判断是否是可以上传的类型，这种方式称为前台脚本检测扩展名
* 绕过前台脚本检测扩展名，就是将所要上传文件的扩展名更改为符合脚本检测规则的扩展名，通过BurpSuite工具，截取数据包，并将数据包中文件扩展名更改回原来的，达到绕过的目的

过程：
* 打开火狐，设置代理为127.0.0.1:8080 

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/1.JPG)

* 打开burp，点击proxy下的intercept，intercept is off，同样要在options标签设置代理，端口与浏览器设置一致 

![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/2.JPG)

* 绕过验证
    - 在火狐输入地址http://ip:8080/up,进入文件上传漏洞演示脚本，点击绕过前台脚本检测扩展名上传Webshell

    ![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/3.JPG)

    - 点击浏览，上传木马，后缀为php，前台显示错误窗口

    ![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/4.JPG)

    - 打开burp打开抓包，将php后缀修改为jpg后重新上传文件
    ![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/5.JPG)

    ![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/6.JPG)

    - Burp抓到包后，将文件后缀修改为php

    ![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/7.JPG)

    - 点击forward，发送数据包，前台提示lubr.php上传成功

    ![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/8.JPG)

* 连接木马
    - 连接木马，在浏览器输入木马的完整地址http://192.168.1.3:8080/up/uploads/lubr.php，网页没有错误提示

    ![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/9.JPG)

    - 打开中国菜刀，添加木马地址，http://192.168.1.3:8080/up/uploads/lubr.php,密码lubr,选择脚本类型php(eval)，点击添加

    ![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/10.JPG)

    - 双击添加记录，即可查看完整目录

    ![](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E7%BB%95%E8%BF%87%E5%89%8D%E5%8F%B0%E8%84%9A%E6%9C%AC%E6%A3%80%E6%B5%8B%E6%89%A9%E5%B1%95%E5%90%8D%E4%B8%8A%E4%BC%A0webshell/11.JPG)

    