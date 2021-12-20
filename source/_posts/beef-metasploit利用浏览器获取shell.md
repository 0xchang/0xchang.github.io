---
title: beef+metasploit利用浏览器获取shell
date: 2021-12-20 19:32:48
tags: beef msf shell
---

## beef+metasploit利用浏览器获取shell

### 利用beef获得目标主机shell

#### 原理：beef是一个用于合法研究和测试目的的专业浏览器漏洞利用框架。它允许有经验的渗透测试人员或系统管理员对目标进行攻击测试。攻击成功以后会加载浏览器劫持会话。BEEF可以利用XSS跨站脚本漏洞。

### 过程：

打开终端，将beef和metasploit进行关联，终端进入`beef-xss`目录，编辑文件`config.yaml`

![image-20211220195046684](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220195046684.png)

将文件中metasploit的`enable`的`false`修改为`true`,保存退出

![image-20211220195427947](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220195427947.png)

进入 /usr/share/beef-xss/extensions/metasploit/目录，编辑`config.yaml`文件，把`host`的参数跟`callback_host`参数修改成kali主机ip

![image-20211220195701762](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220195701762.png)

![image-20211220195837527](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220195837527.png)

将`custom path`的路径修改为 `/usr/share/metasploit-framework/`，保存退出

![image-20211220200034953](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220200034953.png)

输入命令`/etc/init.d/postgresql start`，启动postgresql服务，输入命令`msfdb init`，初始化数据库

![image-20211220200335286](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220200335286.png)

终端中输入msfconsole，启动metasploit，输入`load msgrpc ServerHost=ip Pass=abc123`

![image-20211220200632568](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220200632568.png)

打开新终端，输入cd /usr/share/beef-xss/，切换到beef-xss目录下，输入`./beef –x`加载模块

![image-20211220200906579](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220200906579.png)

打开浏览器，输入`http://ip:3000/ui/panel`，访问`beef`，账号密码都是`beef`

![image-20211220201119958](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220201119958.png)

在靶机访问`http://ip:3000/demos/butcher/index.html`

![image-20211220201335685](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220201335685.png)

返回kali,看到beef自动连接客户端，提示目标已上线

![image-20211220201458346](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220201458346.png)

单击目标ip,`Details`标签显示了客户端主机详细信息，如火狐浏览器，IE6，浏览器支持flash，支持vbscript脚本，操作系统类型

![image-20211220201907661](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220201907661.png)

单击`commands`标签，显示插件

![image-20211220202003645](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220202003645.png)

终端进入/var/www/html（apache2默认网站目录）,编辑一个`xss.html`文件，自定义xss.html网页

![image-20211220202448920](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220202448920.png)

终端修改/etc/apache2/ports.conf，修改默认端口号为8080，保存退出

![image-20211220202622345](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220202622345.png)

终端启动`service apache2 start`，没有显示表示启动成功

![image-20211220202805838](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220202805838.png)

靶机输入http://192.168.1.2:8080/xss.html访问网页

![image-20211220202925168](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220202925168.png)

返回kali，刷新网页，单击目标ip，点击commans,依次选择`Browser`,`Hooked Domain`,`Redirect Browser`,单击最右侧的`RedirectURL`,输入http://192.168.1.2:3000/demos/butcher/index.html,但单击`Execute`，目标主机自动访问此网页

![image-20211220203358764](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220203358764.png)

![image-20211220203430757](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220203430757.png)

单击commands标签，选择`Social Engineering`,`Pretty Theft`,最右侧选择框模板弹出，选择facebook模板，窃取账号密码，单击Execute，执行攻击

![image-20211220203726251](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220203726251.png)

靶机弹出facebook登录对话框，输入账号密码就会被窃取

![image-20211220203846921](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220203846921.png)

在beef的`Module Result History`标签页中，单击历史记录，发现窃取到账号密码

![image-20211220203919754](https://gitee.com/oxchang/img-host/raw/master/beef-metasploit利用浏览器获取shell/image-20211220203919754.png)

实验完成
