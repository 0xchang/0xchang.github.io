---
title: 进程间Messenger通信
date: 2021-12-27 21:49:40
tags: 
- 进程 
- Messenger 
---
## 进程间Messenger通信

目的：

* 通过JAVA编程理解Android进程通信

原理：

* Android程序中的进程间数据传递的问题到许多机制，比如aidl, Messenger, 以及Intent, ContentProvider以及底层的binder。Messenger是Android提供的一个工具类。使用Messenger可以避免编写AIDL文件来进行进程间通信，简化进程间通信的功能。Messenger，信使，其指向一个Handler，他人可以使用信使向Handler发送消息。信使实现了基于消息队列的跨进程的通讯，在一个进程中 创建一个指向Handler的信使，然后把信使返回给其他的进程，使得其它的进程可以向这个进程发送消息。在Messenger内部有一个 IMessenger接口指针，其在Messenger的构造函数中指向了一个Handler中的IMessenger，这样就保存了一个指向 Handler的指针。
* 基本原理：服务端的Messenger需要在onBind方法中返回IBinder实例：

```java
    @Override
    public IBinder onBind(Intent intent) {
       return messenger.getBinder();
    }
```

* 客户端则需要在如下代码中创建与之关联的客户端Messenger：

```java
       @Override
       public void onServiceConnected(ComponentName name, IBinder service) {
           // 得到了一个binder作为桥梁，创建客户端的信使
           messenger = new Messenger(service);
        }
```

* 两边的信使通过IBinder实例关联起来后，就可以通过send方法来互相发送消息了。 如果需要回调，则将另一个Messenger放到Message的replayTo属性中发送给另一端，由另一端来执行回调函数。

过程：


打开eclipse，点击file-import，弹出import窗口

选择android下的existing android code into workspace![image-20211229103119506](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E8%BF%9B%E7%A8%8B%E9%97%B4Messenger%E9%80%9A%E4%BF%A1/image-20211229103119506.png)

点击next，弹出导入窗口，接着点击browse，选择simpleedu31![image-20211229103215577](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E8%BF%9B%E7%A8%8B%E9%97%B4Messenger%E9%80%9A%E4%BF%A1/image-20211229103215577.png)

点击finish导入

打开avd虚拟机simpleedu31，右键项目，选择run as-android application![image-20211229103526330](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E8%BF%9B%E7%A8%8B%E9%97%B4Messenger%E9%80%9A%E4%BF%A1/image-20211229103526330.png)

程序运行![image-20211229104428625](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E8%BF%9B%E7%A8%8B%E9%97%B4Messenger%E9%80%9A%E4%BF%A1/image-20211229104428625.png)

点击bind,启动messengerservice![image-20211229104450082](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E8%BF%9B%E7%A8%8B%E9%97%B4Messenger%E9%80%9A%E4%BF%A1/image-20211229104450082.png)

点击message向MessengerService发送消息，messengerservice收到消息后弹出显示框![image-20211229104520678](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E8%BF%9B%E7%A8%8B%E9%97%B4Messenger%E9%80%9A%E4%BF%A1/image-20211229104520678.png)

点击callback，messengerservice收到消息后会构造一个信息返回给mainactivity![image-20211229104539221](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/%E8%BF%9B%E7%A8%8B%E9%97%B4Messenger%E9%80%9A%E4%BF%A1/image-20211229104539221.png)

点击unbind停止messengerservice服务，messengerservice服务停止之后点击message，callback，均不会有反应

