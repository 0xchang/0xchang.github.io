---
title: APP加壳
date: 2021-12-27 16:40:29
tags: 
- APP 
- 加壳
---
## APP加壳
目的：
* 熟悉常用Android编译工具的使用

原理：
* APP加壳是在二进制的程序中植入一段代码，在程序的外面再包裹上一段代码，在运行的时候优先取得程序的控制权，保护里面的代码不被非法修改或反编译。

* AndroidDex文件加壳涉及到三个程序：①加壳程序：对源Apk进行加密和脱壳项目的Dex的合并；②脱壳程序：解密壳数据；③源程序：需要加壳处理的被保护代码。

* APP加壳的优势在于保护核心代码,提高破解的难度,还可以缓解代码注注入攻击。缺点是影响程序的运行效率。

过程：

导入项目ForceApkObj,点击源程序项目。![image-20211229085141078](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229085141078.png)

右键项目，选择`Android Tools`,`Export Signed Application Package`![image-20211229085259329](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229085259329.png)

默认，点击next![image-20211229085322221](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229085322221.png)

选择`create keystore`,点击browse选择目录，密码随便设（123456），填完参数点击next![image-20211229085707268](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229085707268.png)

创建密钥信息（SimpleEdu11;123456;123456;100;SimpleEdu），点击next![image-20211229085847492](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229085847492.png)

选择APK生成位置，点击Browse，默认为SimpleEdu11![image-20211229085932751](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229085932751.png)

点击finish完成操作![image-20211229085956125](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229085956125.png)

成功生成源程序APK文件![image-20211229090057678](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229090057678.png)

右击脱壳程序项目ReforceApk，点击`Android Tools`，`Export UnSigned Application Package`![image-20211229090206565](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229090206565.png)

接下来的步骤和源程序相同，生成脱壳程序APK![image-20211229090304049](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229090304049.png)

右击脱壳程序apk，解压到…，得到解压文件

修改dex文件的名称为ForceApkObj.dex![image-20211229091116540](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229091116540.png)

将FoceApkOBj.dex文件与ForceApkObj.apk文件放在C:\AndroidSec\SimpleEdu11\DexShellTools\force，如下为加壳程序的代码，它的作用是将源程序APK和脱壳程序dex文件合成新的dex文件![image-20211229091826511](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229091826511.png)

点击按键![image-20211229091948817](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229091948817.png)

选择`Debug As`，`2`![image-20211229092002112](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229092002112.png)

成功运行![image-20211229092026132](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229092026132.png)

按照导入的文件路径，找到生成的dex文件![image-20211229092046674](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229092046674.png)

右击脱壳程序，用压缩工具打开![image-20211229092237838](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229092237838.png)

将class文件替换脱壳程序中的dex文件![image-20211229092303091](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229092303091.png)

重新签名，将之前替换好的APK放入`APK重签名`文件中![image-20211229092406804](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229092406804.png)

双击签名程序后生成NewApk.apk![image-20211229092513149](https://gitee.com/oxchang/img-host/raw/master/APP%E5%8A%A0%E5%A3%B3/image-20211229092513149.png)

