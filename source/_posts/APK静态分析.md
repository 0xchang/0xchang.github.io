---
title: APK静态分析
date: 2021-12-28 08:22:15
tags: 
- APK
- 逆向
- IDA
---


## APK静态分析

目的：
* 了解APK（DEX）静态分析的基本步骤
* 掌握使用IDApro辅助进行静态分析的方法

原理：
* 程序静态分析（Program Static Analysis）是指在不运行代码的方式下，通过词法分析、语法分析、控制流、数据流分析等技术对程序代码进行扫描，验证代码是否满足规范性、安全性、可靠性、可维护性等指标的一种代码分析技术。目前静态分析技术向模拟执行的技术发展以能够发现更多传统意义上动态测试才能发现的缺陷，例如符号执行、抽象解释、值依赖分析等等并采用数学约束求解工具进行路径约减或者可达性分析以减少误报增加效率。
* Smali，Baksmali分别是指安卓系统里的Java虚拟机（Dalvik）所使用的一种.dex格式文件的汇编器，反汇编器。

过程：

使用解压软件打开crackme0502.apk,解压出classes.dex，打开IDApro并将classes.dex拖入到IDA中![image-20211229105021723](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229105021723.png)

导入结构化文件，点击file-script file，然后选择dex.idc文件，确定![image-20211229105052129](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229105052129.png)

按ALT+Q，可以看到DEX文件的结构，选择某一项点击确定，可以跳到相应字段的起始位置![image-20211229105232030](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229105232030.png)

打开eclipse，启动SimpleEdu45虚拟机，并在SimpleEdu45目录下运行命令adb install crackme0502.apk![image-20211229105658041](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229105658041.png)

打开crackeme0502，点击“获取注解”按钮![image-20211229105844377](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229105844377.png)

打开crackme0502，点击“检测注册码”按钮![image-20211229105858622](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229105858622.png)

切换回IDA pro，切换至Exports选项卡。程序运行的主Activity一般叫MainActivity，所以按住ctrl+f，键入Main，可以发现MainActivity中有两个内部类都有OnClick()方法。![image-20211229110001643](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229110001643.png)

分别双击MainActivity\$1.onClick@VL和MainActivity\$2.onClick@VL来查看对应的方法实现，通过对比，可以发现后者的方法创建了名为SNChecker的一个对象，故序列号检查功能应该在MainActivity$​2.onClick@VL中。


MainActivity$1.onClick@VL ![image-20211229110055781](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229110055781.png)

MainActivity$2.onClick@VL ![image-20211229110114049](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229110114049.png)

按空格键将IDA切换至“流程视图”![image-20211229110629336](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229110629336.png)

很明显可以看出，红色的线是不满足条件所跳转到的语句，绿色的是满足条件时的跳转。所以把if-eqz换成if-nez就有可能破解成功。故将光标定位值if-eqz一行![image-20211229110655907](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229110655907.png)

然后切换至HEX-view 1选项卡![image-20211229110748059](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229110748059.png)

关闭IDA，右键使用UltraEdit工具打开classes.dex，按CTRL+F搜索38020F00（有多个38020F00，注意起始地址是否为002D0B0，如下图所示）。由于if-nez的Opcode为39，故将38改为39，然后保存该文件（点击右上角切为16进制显示模式）![image-20211229111128616](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229111128616.png)

ctrl+f寻找,将38改为39

运行DexFixer.exe，将保存好的DEX文件拖进窗口中，进行Header的Checksum修复![image-20211229111244001](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229111244001.png)

把classes.dex重新添加至apk文件（使用360压缩打开apk文件将class.dex替换即可），并运行命令signapk crackme0502.apk重新签名![image-20211229111424498](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229111424498.png)

并把生成的signed.apk按着之前的方法重新安装至虚拟机中（需要先将之前安装的程序卸载）adb install signed.apk。注意是signed.apk。再次点击下面的按钮，发现软件提示已注册。

![image-20211229111717277](http://121.5.125.62:88/image/APK%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90/image-20211229111717277.png)

