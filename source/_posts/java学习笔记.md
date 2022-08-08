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
字符类型初始化为'\u0000',空格
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
