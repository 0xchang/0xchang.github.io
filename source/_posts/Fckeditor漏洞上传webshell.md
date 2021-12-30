---
title: Fckeditor漏洞上传webshell
date: 2021-12-27 08:45:33
tags: -fckeditor
      -webshell
---

目的：Fckeditor为在线网页编辑器。本实验演示`Fckeditor2.4.2`以下版本的一个编辑器上传漏洞。

原理：Fckeditor在2.4.2以下存在一个直接上传任意文件的上传页面，可直接上传webshell。

过程：

打开网站`http://ip:8001/fckeditor`,判断是否有fckeditor编辑器，出现403禁止访问，说明此目录存在
![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/1.JPG)

判断fckeditor编辑器版本号，输入`http://ip:8001/FCKeditor/_whatsnew.html`,返回页面可知fckeditor版本为2.0

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/2.JPG)

此版本的fckeditor存在两个上传漏洞页面：
`FCKeditor/editor/filemanager/browser/default/browser.html?type=Image&connector=connectors/asp/connector.asp`
`FCKeditor/editor/filemanager/browser/default/connectors/asp/connector.asp?Command=GetFoldersAndFiles&Type=zhang&CurrentFolder=/`

第一个页面实在网站根目录下的`userfiles`目录下的`Image`目录下打开一个上传页面，上传的文件都保存在这个目录下，第二是在网站跟了目录下创建一个zhang目录

http://ip:8001/FCKeditor/filemanager/browser/default/browser.html?type=Image&connector=connectors/asp/connector.asp

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/3.JPG)

先上传一个正常图片并查看返回结果，上传图片为1.jpg

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/4.JPG)

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/5.JPG)

重新上传一个asp一句话，不允许上传
 
![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/6.JPG)

尝试使用00截断的方法再次上传，打开burpsuite

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/7.JPG)

火狐浏览器设置代理，为127.0.0.1:8080

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/8.JPG)

返回浏览器，重新上传文件，burp抓取到刷新的数据包，多次点击`forward`，直到浏览器显示页面

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/9.JPG)

切换到浏览器，将2.asp改为2.asp.jpg，并上传

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/10.JPG)

此时burp会拦截上传的数据包

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/11.JPG)

点击`HEX`，切换hex模式,修改此处内容

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/12.JPG)

将`2e`修改为`00`

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/13.JPG)

切换为raw，点击forward，发送请求，再次刷新上传页面，发现asp文件已经上传成功

![](https://gitee.com/oxchang/img-host/raw/master/Fckeditor%E6%BC%8F%E6%B4%9E%E4%B8%8A%E4%BC%A0webshell/14.JPG)


