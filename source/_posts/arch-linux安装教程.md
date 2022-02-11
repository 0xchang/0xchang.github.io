---
title: arch-linux安装教程
date: 2022-02-03 15:27:10
tags: 
- arch
- linux
---

## vmware安装arch

### 0x01 下载镜像源

* 下载镜像源[清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/)，下载合适的镜像源

### 0x02 vmware新建虚拟机

* vmware新建虚拟机，然后选择镜像开机，网络选择NAT

### 0x03 开始安装

* 默认选择第一项

![image-20220203153912752](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203153912752.png)

* 一般默认开启ssh服务，建议用ssh链接可以复制粘贴比比较方便![image-20220203154610491](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203154610491.png)如果没有开启ssh可以使用**systemctl start sshd**开启服务，使用**passwd**修改密码之后使用**ip a**查看ip地址之后使用ssh连接![image-20220203155023782](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203155023782-16438746240531.png)

* 测试网络连通性![image-20220203155831482](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203155831482.png)

* 磁盘分区：运行命令**parted /dev/sda** ![image-20220203183147074](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203183147074.png)

  创建主引导记录分区表**mklabel msdos**,依次输入以下命令

  ```shell
  mkpart primary ext4 1M 100M
  set 1 boot on
  mkpart primary linux-swap 100M 4G
  mkpart primary ext4 4G 100%
  print
  ```

  ![image-20220203193804330](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203193804330.png)

  使用**quit**退出程序

  

* 磁盘格式化，查看分区情况**lsblk /dev/sda**，输入以下命令进行格式化操作

  ```shell
  mkfs.ext4 /dev/sda1
  mkfs.ext4 /dev/sda3
  mkswap /dev/sda2
  swapon /dev/sda2
  ```

  ![image-20220203194117259](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203194117259.png)

  然后进行挂载

  ```shell
  mount /dev/sda3 /mnt
  mkdir –p /mnt/boot
  mount /dev/sda1 /mnt/boot
  ```

  

* 设置安装镜像源

  使用nano或者vim编辑器编辑对应文件

  ```shell
  nano /etc/pacman.d/mirrorlist
  ```

  文件最前方加入**Server	=	http://mirrors.163.com/archlinux/\$repo/os/\$arch**,**ctrl+o**保存，**ctrl+x**退出

  ![image-20220203194655988](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203194655988.png)

* 安装软件包

    ```shell
    pacstrap -i /mnt base linux linux-firmware dhcpcd vim openssh xfsprogs man net-tools base-devel
    ```

    遇到选项直接回车

* 配置文件系统

  对分区表进行设置**genfstab -U -P /mnt >> /mnt/etc/fstab**![image-20220203195507094](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203195507094.png)

* chroot进入新系统

  ```shell
  arch-chroot /mnt /bin/bash
  ```

  运行后，我们就将切入安装的操作系统之中

* 修改系统的字符编码支持

  ```shell
  vim /etc/locale.gen
  ```

  将对应文件内容前面的#去掉:

  en_US.UTF-8 UTF-8 

  zh_CN.UTF-8UTF-8

  zh_CN GB2312

  之后输入命令,使用en_US.UTF-8作为默认系统编码

  ```shell
  locale-gen
  echo LANG=en_US.UTF-8 > /etc/locale.conf
  ```

  

* 设置时区

  ```shell
  ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
  ```

  

* 设置root密码

  ```shell
  passwd 
  ```

* 创建 ramdisk

  ```shell
  mkinitcpio -p linux
  ```

* 安装grub

  ```shell
  pacman -S grub os-prober #安装grub包
  grub-install /dev/sda #安装grub
  grub-mkconfig -o /boot/grub/grub.cfg #配置grub
  ```

  ![image-20220203200708506](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203200708506.png)

* 配置网络和主机名

  ```shell
  echo arch > /etc/hostname #arch可以改成自己喜欢的
  systemctl enable dhcpcd.service #如果是有线路由，设置开机自动联网
  pacman -S iw wpa_supplicant dialog #无线网络安装软件包
  ```

* 创建普通用户arch

  ```shell
  useradd -m -G wheel -s /bin/bash arch #创建arch并添加到wheel组
  passwd arch #修改arch密码
  visudo #修改sudo相关配置，去掉相关位置#即可
  ```

  ![image-20220203201447697](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203201447697.png)

* **exit**退出chroot环境，**reboot**重启系统

  输入用户名登录进入系统成功

  ![image-20220203201653721](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203201653721.png)

### 0x04安装yay（可有可无）

  ```shell
  vim /etc/pacman.conf,最后一行添加以下数据
  [archlinuxcn]
  SigLevel = Never
  Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
  [blackarch]
  SigLevel = Never
  Server = https://mirrors.tuna.tsinghua.edu.cn/blackarch/$repo/os/$arch
  ```

  然后更新

  ```shell
  pacman -Syyu
  #接着安装yay
  pacman -S yay
  ```

  ![image-20220203203808157](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/arch-linux安装教程/image-20220203203808157.png)

  安装完成

