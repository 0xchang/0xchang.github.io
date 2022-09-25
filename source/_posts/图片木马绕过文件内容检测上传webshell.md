---
title: 图片木马绕过文件内容检测上传webshell
date: 2021-12-27 15:04:38
tags: 
- webshell
- 图片木马
---

## 图片木马绕过文件内容检测上传webshell

目的：
* 理解绕过内容验证上传的原理
* 学习绕过内容验证上传的过程

原理
* 一般文件内容验证使用getimagesize()函数检测，会判断文件是否是一个有效的文件图片，如果是，则允许上传，否则的话不允许上传
* 本实例就是将一句话木马插入到一个【合法】的图片文件当中，然后用中国菜刀远程连接

过程：

* 打开火狐，进入网址http://192.168.1.3:8080/up，进入构造图片马绕过上传webshell![image-20211228104158522](https://121.5.125.62:88/image/%E5%9B%BE%E7%89%87%E6%9C%A8%E9%A9%AC%E7%BB%95%E8%BF%87%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%A3%80%E6%B5%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228104158522.png)
* 选择上传木马，后缀php改为php.jpg
* 点击上传，提示错误
* ![image-20211228104300084](https://121.5.125.62:88/image/%E5%9B%BE%E7%89%87%E6%9C%A8%E9%A9%AC%E7%BB%95%E8%BF%87%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%A3%80%E6%B5%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228104300084.png)
* 制作图片马，将要上传的木马与图片一个文件下![image-20211228104350818](https://121.5.125.62:88/image/%E5%9B%BE%E7%89%87%E6%9C%A8%E9%A9%AC%E7%BB%95%E8%BF%87%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%A3%80%E6%B5%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228104350818.png)
* 打开cmd，进入对应目录![image-20211228104423625](https://121.5.125.62:88/image/%E5%9B%BE%E7%89%87%E6%9C%A8%E9%A9%AC%E7%BB%95%E8%BF%87%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%A3%80%E6%B5%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228104423625.png)
* 输入copy pic.jpg/b+lubr.php/a PicLubr.jpg，将lubr.php插入到pic.jpg中![image-20211228104518932](https://121.5.125.62:88/image/%E5%9B%BE%E7%89%87%E6%9C%A8%E9%A9%AC%E7%BB%95%E8%BF%87%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%A3%80%E6%B5%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228104518932.png)
* 在火狐上传PicLubr.jpg，后缀名改为PicLubr.jpg.php，然后上传![image-20211228104707308](https://121.5.125.62:88/image/%E5%9B%BE%E7%89%87%E6%9C%A8%E9%A9%AC%E7%BB%95%E8%BF%87%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%A3%80%E6%B5%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228104707308.png)
* 输入连接访问木马文件![image-20211228104737829](https://121.5.125.62:88/image/%E5%9B%BE%E7%89%87%E6%9C%A8%E9%A9%AC%E7%BB%95%E8%BF%87%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%A3%80%E6%B5%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228104737829.png)
* 打开中国菜刀，添加木马连接，输入密码lubr，脚本类型php(eval)
* 双击添加的shell，即可访问网站的完整目录![image-20211228104832489](https://121.5.125.62:88/image/%E5%9B%BE%E7%89%87%E6%9C%A8%E9%A9%AC%E7%BB%95%E8%BF%87%E6%96%87%E4%BB%B6%E5%86%85%E5%AE%B9%E6%A3%80%E6%B5%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228104832489.png)

