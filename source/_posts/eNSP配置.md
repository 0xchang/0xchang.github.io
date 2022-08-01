---
title: eNSP配置
date: 2022-07-26 10:39:03
tags:
- eNSP
- 华为
---

## 路由器
### 路由器设置ip
```
<Huawei>system-view           //进入系统视图
[Huawei]display interface brief     //显示接口信息
[Huawei]interface GigabitEthernet 0/0/0    //进入接口
[Huawei-GigabitEthernet0/0/0]ip address 192.168.1.254 24     //设置ip地址
[Huawei-GigabitEthernet0/0/0]q    //退出
```

### 路由器设置console认证
```
<Huawei>sys
Enter system view, return user view with Ctrl+Z.
[Huawei]aaa
[Huawei-aaa]local-user ccc password cipher 123
[Huawei-aaa]local-user ccc service-type terminal //设置ccc为终端用户
[Huawei-aaa]q
[Huawei]user-interface console 0  //进入console接口
[Huawei-ui-console0]authentication-mode aaa  //验证模式改为aaa
```

### 路由器设置telnet远程登录
```
<Huawei>system-view
[Huawei]aaa     //进入aaa视图
[Huawei-aaa]local-user bbb password cipher 123    //创建用户bbb，设置密码为123
[Huawei-aaa]local-user bbb privilege level 15     //设置用户权限为15
[Huawei-aaa]local-user bbb service-type telnet    //设置用户服务类型telnet
[Huawei-aaa]q             //退出
[Huawei]user-interface vty 0 4      //设置5个人访问路由器
[Huawei-ui-vty0-4]authentication-mode aaa
```
### 路由器设置ssh远程登录
```
---服务机
[Huawei]stelnet server enable         //开启ssh服务，默认关闭
[Huawei]rsa local-key-pair create     //创建rsa秘钥
Input the bits in the modulus[default = 512]:1024
[Huawei]aaa
[Huawei-aaa]local-user ccc password cipher 123
[Huawei-aaa]local-user ccc privilege level 15
[Huawei-aaa]local-user ccc service-type ssh
[Huawei-aaa]q
[Huawei]user-interface vty 0 4
[Huawei-ui-vty0-4]authentication-mode aaa
[Huawei-ui-vty0-4]protocol inbound ssh   //开启vty线路的ssh访问功能
[Huawei-ui-vty0-4]q
[Huawei]ssh user ccc authentication-type all   //定义ssh的认证方式
---客户机
[Huawei]ssh client first-time enable    //第一次开启客户端
[Huawei]stelnet 10.1.1.1
```
### 设置空闲时间
```
<Huawei>system-view
[Huawei]user-interface console 0         //进入console 0
[Huawei-ui-console0]idle-timeout 10      //设置空闲时间10分钟
```
### 设置ACL规则
* 高级ACL（简单）
```
[Huawei]acl 3000            //进入高级acl设置，编号3000
[Huawei-acl-adv-3000]rule 1 deny ip source 192.168.1.1 0 destination 172.20.1.1 0   //0为通配符掩码
[Huawei-acl-adv-3000]q
[Huawei]interface GigabitEthernet 0/0/0
[Huawei-GigabitEthernet0/0/0]traffic-filter inbound acl 3000  //绑定到接口0/0/0 入口方向
```
* 基本ACL
```
[Huawei]acl 2000            //基本ACL设置，编号2000
[Huawei-acl-basic-2000]rule 1 deny source 192.168.1.2 0   //拒绝192.168.1.2流量
[Huawei-acl-basic-2000]rule 2 permit source 192.168.1.1 0   //允许192.168.1.1流量
[Huawei-acl-basic-2000]rule permit   //全部允许
[Huawei-acl-basic-2000]q
[Huawei]interface GigabitEthernet 0/0/1
[Huawei-GigabitEthernet0/0/1]traffic-filter outbound acl  2000   //绑定到0/0/1的出口方向
```
