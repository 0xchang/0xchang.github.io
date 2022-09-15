---
title: 绕过content_type检测文件类型上传webshell
date: 2021-12-27 14:15:40
tags: 
- content_type
- webshell
---

## 绕过content_type检测文件类型上传webshell

目的:
* 理解绕过Content-Type检测文件类型上传的原理
* 学习绕过Content-Type检测文件类型上传的过程

原理:
* 当浏览器在上传文件到服务器的时候，服务器对说上传文件的Content-Type类型进行检测，如果是白名单允许的，则可以正常上传，否则上传失败。
* 绕过Content--Type文件类型检测，就是用BurpSuite截取并修改数据包中文件的Content-Type类型，使其符合白名单的规则，达到上传的目的。

过程：

* 打开火狐浏览器，修改代理127.0.0.1:8080![image-20211228103046467](http://121.5.125.62:88/image/%E7%BB%95%E8%BF%87content-type%E6%A3%80%E6%B5%8B%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228103046467.png)

* 打开burp，点击intercept is off![image-20211228103152265](http://121.5.125.62:88/image/%E7%BB%95%E8%BF%87content-type%E6%A3%80%E6%B5%8B%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228103152265.png)
* 打开浏览器,输入http://192.168.1.3:8080/up,进入文件上传漏洞演示脚本，点击content_type检测文件类型上传webshell。![image-20211228103256712](http://121.5.125.62:88/image/%E7%BB%95%E8%BF%87content-type%E6%A3%80%E6%B5%8B%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228103256712.png)
* 上传php文件，前台显示错误窗口![image-20211228103342121](http://121.5.125.62:88/image/%E7%BB%95%E8%BF%87content-type%E6%A3%80%E6%B5%8B%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228103342121.png)
* Burp开启抓包
* Burp抓到的数据包,在数据包中的content-type的application-stream修改为image/gif![image-20211228103504817](http://121.5.125.62:88/image/%E7%BB%95%E8%BF%87content-type%E6%A3%80%E6%B5%8B%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228103504817.png)
* 点击forward，发送数据包，前台提示上传lubr.php成功![image-20211228103538500](http://121.5.125.62:88/image/%E7%BB%95%E8%BF%87content-type%E6%A3%80%E6%B5%8B%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228103538500.png)
* 浏览器输入地址http://192.168.1.3:8080/up/uploads/lubr.php，页面没有错误提示![image-20211228103621026](http://121.5.125.62:88/image/%E7%BB%95%E8%BF%87content-type%E6%A3%80%E6%B5%8B%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228103621026.png)
* 打开中国菜刀，添加木马地址，密码，lubr，脚本类型php(eval)，点击添加![image-20211228103730160](http://121.5.125.62:88/image/%E7%BB%95%E8%BF%87content-type%E6%A3%80%E6%B5%8B%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228103730160.png)
* 双击添加记录，可查看完整目录![image-20211228103747989](http://121.5.125.62:88/image/%E7%BB%95%E8%BF%87content-type%E6%A3%80%E6%B5%8B%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B%E4%B8%8A%E4%BC%A0webshell/image-20211228103747989.png)

