---
title: cisco配置
date: 2022-08-01 09:19:00
tags:
- cisco
- 路由器
---

## 路由器
### 设置console密码
```cisco
Router>enable                     //特权模式
Router#configure terminal         //全局模式
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#line console 0     //进入0号控制台
Router(config-line)#password 111  //设置密码111
Router(config-line)#login         //开启密码
```
![console 密码](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/cisco-%E8%B7%AF%E7%94%B1%E5%99%A8%E8%AE%BE%E7%BD%AE%E7%94%A8%E6%88%B7%E5%90%8D%E5%92%8C%E5%AF%86%E7%A0%81/console密码.JPG)

### 设置特权模式密码
```cisco
Router>enable
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#enable password 111  //设置明文密码
Router(config)#enable secret 222    //设置加密密码，会导致password失效
Router(config)#login                //开启密码
```
![特权密码](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/cisco-%E8%B7%AF%E7%94%B1%E5%99%A8%E8%AE%BE%E7%BD%AE%E7%94%A8%E6%88%B7%E5%90%8D%E5%92%8C%E5%AF%86%E7%A0%81/特权密码.JPG)

### 设置远程登录telnet密码
```cisco
Router>enable
Router#configure terminal
Router(config)#line vty 0 4         //设置0-4（共5个）用户可以进行Telnet登录
Router(config-line)#password 111    //设置密码111
Router(config-line)#login local     //开启密码
```
![telnet 密码](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/cisco-%E8%B7%AF%E7%94%B1%E5%99%A8%E8%AE%BE%E7%BD%AE%E7%94%A8%E6%88%B7%E5%90%8D%E5%92%8C%E5%AF%86%E7%A0%81/telnet%20%E5%AF%86%E7%A0%81.JPG)

### 设置远程登录telnet用户名和密码
* 登录权限为普通

```cisco
Router>enable
Router#configure terminal
Router(config)#username aaa password cisco   //非加密
Router(config)#username aaa secret cisco     //加密
Router(config)#line vty 0 4
Router(config-line)#login  local
```
![telnet-特权模式](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/cisco-%E8%B7%AF%E7%94%B1%E5%99%A8%E8%AE%BE%E7%BD%AE%E7%94%A8%E6%88%B7%E5%90%8D%E5%92%8C%E5%AF%86%E7%A0%81/telnet普通权限.JPG)

### 设置远程登录telnet用户名和密码（特权模式）
* 登录后为特权模式

```cisco
Router>enable
Router#configure terminal
Router(config)#username ddd privilege 15 secret ddd
Router(config)#line vty 0 4
Router(config-line)#login  local
Router(config-line)#no password     //取消telnet远程登录
Router(config-line)#no login        //不进行密码检查
```
![telnet-user-password](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/cisco-%E8%B7%AF%E7%94%B1%E5%99%A8%E8%AE%BE%E7%BD%AE%E7%94%A8%E6%88%B7%E5%90%8D%E5%92%8C%E5%AF%86%E7%A0%81/telnet-user-password.JPG)

### 登录超时自动断开
```cisco
Router>enable
Router#configure terminal
Router(config)#line vty 0 4
Router(config-line)#exec-timeout 20 0    //20分钟0秒自动断开
```

### 登录次数限制与时间锁定
```cisco
Router>enable
Router#configure terminal
Router(config)#login block-for 1800 attempts 10 within 600    //600秒内登录错误10次，锁定1800秒
```

### 配置ssh登录
```cisco
Router>enable
Router#configure terminal
Router(config)#hostname router1  
router1(config)#ip domain-name neoshi.net  //必须要先设置hostname和domain才能进行下一步
router1(config)#crypto key generate rsa
The name for the keys will be: router1.neoshi.net
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024    //生成1024rsa秘钥，支持360-2048
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

Router(config)#username aaa secret aaa
Router(config)#ip ssh authentication-retries 4   //设置ssh认证次数为4，可以为0-5之间
router1(config)#line vty 0 4
*3? 1 0:14:8.8:  %SSH-5-ENABLED: SSH 1.99 has been enabled
router1(config-line)#login local
router1(config-line)#transport input ssh     //设置登录模式为ssh，默认为all
```

### privilege等级介绍
#### privilege等级切换
* privilege一共有15个等级，没有enable密码的情况下，不能从低等级切换到高等级，否则会报错
```
router1>en 2
 % Error in authentication.
 ```
* 加入需要从en 0切换到en 3 ，需要回到en 15 为en 3 设置一个密码，然后进行切换
```
router1>en 15
Router1#configure terminal
router1(config)#enable secret level 3 a  //为en 3 设置密码为a
router1>en 3
Password:
router1#           //不同的级别使用的命令会不一样
```

### 配置日志服务器
```
Router>enable
Router#configure terminal
router1(config)#logging on                 //打开日志服务
router1(config)#logging trap debugging     //设置等级为debugging
router1(config)#logging x.x.x.x           //设置日志服务器地址
router1(config)#service timestamps log datetime  //设置时间戳
```

### 路由器配置
```
Router>enable
Router#configure terminal
Router(config)#interface FastEthernet0/0
Router(config-if)#ip address 10.1.1.254 255.255.255.0  //绑定地址
Router(config-if)#no shutdown    //启动接口
Router(config-if)#ip route 目的地址 子网掩码 路由器下一跳      //设置静态路由
```

### ACL配置
#### 标准ACL
* 顺序匹配原则
* 标准访问控制列表因为只能限制源IP地址，因此应该把ACL放到离目标最近的端口出方向上。
```
Router>enable
Router#configure terminal
Router(config)#access-list 1 deny 192.168.1.1   //拒绝192.168.1.1流量
Router(config)#access-list 1 permit any         //允许任何流量通过
Router(config)#interface fastEthernet 0/1       //进入接口0/1
Router(config-if)#ip access-group 1 out         //设置ACL为出方向
```
#### 扩展ACL
* 扩展ACL可以对数据包中的源、目标IP地址以及端口号进行检查，所以可以将该ACL放置在通信路径中的任一位置。但是，如果放到离目标近的地方，每台路由器都要对数据进行处理，会更多的消耗路由器和带宽资源。放到离源最近的路由器端口入方向直接就将拒绝数据丢弃，可以减少其他路由器的资源占用以及带宽占用。
```
Router>enable
Router#configure terminal
Router(config)#access-list 100 deny tcp host 192.168.1.1 host 172.20.1.1 eq www   //拒绝从192.168.1.1发到172.20.1.1的tcp协议，端口为80
Router(config)#access-list 100 permit ip any any   //允许任何流量
Router(config)#interface fastEthernet 0/0   //设置接口为0/0
Router(config-if)#ip access-group 100 in    //设置ACL为入方向
```
