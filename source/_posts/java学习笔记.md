---
title: java学习笔记
date: 2022-08-03 22:54:13
tags:
- java
---

## Hello world
```java
package hello_world;

public class Main {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println("hello world");

	}
}
```

## 数据类型
### 变量
```java
package datademo1;
public class datatype1 {
	public static void main(String[] args) {
		//byte：8位，最大存储数据量是255，存放的数据范围是-128~127之间。
		//short：16位，最大数据存储量是65536，数据范围是-32768~32767之间。
		//int：32位，最大数据存储容量是2的32次方减1，数据范围是负的2的31次方到正的2的31次方减1。
		//long：64位，最大数据存储容量是2的64次方减1，数据范围为负的2的63次方到正的2的63次方减1。
		//float：32位，数据范围在3.4e-45~1.4e38，直接赋值时必须在数字后加上f或F。
		//double：64位，数据范围在4.9e-324~1.8e308，赋值时可以加d或D也可以不加。
		//boolean：只有true和false两个取值。
		//char：16位，存储Unicode码，用单引号赋值。

		byte a ='1';
		short b = 32767;
		int c=234441;
		//数字后加L
		long d=45646645546546L;
		//数字后加F
		float e=3.2f;
		double f=52.2;
		boolean g=true;
		char h='1';
	}
}
```

### 键盘输入(整数)
```java
package input;
import java.util.Scanner;

public class main {

	public static void main(String[] args) {
		Scanner sc  = new Scanner(System.in);
		int i = sc.nextInt();
		System.out.print(i);
	}

}
```

### 数组
#### 静态初始化
```java
public class Main {
    public static void main(String[] args) {
				//定义数组
        int [] a =new int[]{1,2,3,4,5};
        System.out.println(a);
    }
}
/*
[I@119d7047
[表示数组
I表示类型int
@表示间隔符，固定格式
119d7047真正的地址值
变量可覆盖
*/
```
#### 数组长度
```java
public class Main {
    public static void main(String[] args) {
        int [] a =new int[]{1,2,3,4,5};
        for(int i=0;i<a.length;i++){
            System.out.println(a[i]);
        }
    }
}
//数组名.length得出数组长度
```
#### 动态初始化
```java
public class Main {
    public static void main(String[] args) {
        String[] a = new String[100];
    }
}
/*
整数类型初始化为0
小数类型初始化为0.0
字符类型初始化为'\u0000'
布尔类型 false
引用类型 null
*/
```
## 方法
### 定义
```java
public class Main {
    public static void main(String[] args) {
        int a =10;
        System.out.println(test(a));
    }
    public static boolean test(int num){
        return num>0;
    }
}
```
### 方法重载
```java
/*
类型不同，个数不同，顺序不同
同一个类中
与返回值类型无关
*/
public class Main {
    public static void main(String[] args) {
        System.out.println(add(1,2));
        System.out.println(add(1,2,3));
    }
    public static int add(int a,int b){
        return a+b;
    }
    public static int add(int a,int b,int c){
        return a+b+c;
    }
}
```

## 类
### 初始化
* 一个java文件中可以定义多个class，但只能一个类是public，public的类名称必须和文件名相同，实际开发中建议一个文件定义一个类
### 封装
* 对象代表什么，就得封装对应的数据，并提供数据对应的行为


## 文件
### 文件
```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        File f = new File("test.txt");
        FileOutputStream fop = new FileOutputStream(f);
        // 构建FileOutputStream对象,文件不存在会自动新建
        OutputStreamWriter writer = new OutputStreamWriter(fop, "UTF-8");
        // 构建OutputStreamWriter对象,参数可以指定编码,默认为操作系统默认编码,windows上是gbk
        writer.append("中文输入");
        // 写入到缓冲区
        writer.append("\r\n");
        // 换行
        writer.append("English");
        // 刷新缓存冲,写入到文件,如果下面已经没有写入的内容了,直接close也会写入
        writer.close();
        // 关闭写入流,同时会把缓冲区内容写入文件,所以上面的注释掉
        fop.close();
        // 关闭输出流,释放系统资源
        FileInputStream fip = new FileInputStream(f);
        // 构建FileInputStream对象
        InputStreamReader reader = new InputStreamReader(fip, "UTF-8");
        // 构建InputStreamReader对象,编码与写入相同
        StringBuffer sb = new StringBuffer();
        while (reader.ready()) {
            sb.append((char) reader.read());
            // 转成char加到StringBuffer对象中
        }
        System.out.println(sb.toString());
        reader.close();
        // 关闭读取流

        fip.close();
        // 关闭输入流,释放系统资源
    }
}
```
### 目录
```java
import java.io.File;

public class DirList {
    public static void main(String args[]) {
        String dirname = "/tmp";
        File f1 = new File(dirname);
        if (f1.isDirectory()) {
            System.out.println("目录 " + dirname);
            String s[] = f1.list();
            for (int i = 0; i < s.length; i++) {
                File f = new File(dirname + "/" + s[i]);
                if (f.isDirectory()) {
                    System.out.println(s[i] + " 是一个目录");
                } else {
                    System.out.println(s[i] + " 是一个文件");
                }
            }
        } else {
            System.out.println(dirname + " 不是一个目录");
        }
    }
}
```



### GUI
```java
package top.oxchang.ui;

import javax.swing.JFrame;

public class GameJFrame extends JFrame {

	public GameJFrame() {
		// 设置大小
		this.setSize(800, 600);
		// 设置标题
		this.setTitle("拼图游戏");
		// 设置是否显示
		this.setVisible(true);
	}
}

```
