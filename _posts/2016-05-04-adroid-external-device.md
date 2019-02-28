---
layout: post
title:  Android监听耳机,以及部分外设的插拔
date:   2016-05-04 20:05:00 +0800
categories: Android
tag: 耳机,其他外设插拔
---

* content
{:toc}


> 最近在公司做的项目目需要手机通过充电口插上外设才能使用，项目需求是需要判断接入的外设的VendorId和ProductId，发现使用广播最方便.

#### 1.监听USB外部设备

>首先我门可以通过监听如下三个广播获得自己的设备链接的状态
<br/>`android.hardware.usb.action.USB_STATE` ,
<br/>
`android.hardware.usb.action.USB_DEVICE_ATTACHED` ,
<br/>
`android.hardware.usb.action.USB_DEVICE_DETACHED`


#### 2.监听耳机的状态
首先我门可以通过监听`android.intent.action.HEADSET_PLUG`广播获得自己耳机的插拔,以及耳机是否有话筒进行判断.

> <font color = "red"> 如下代码获取耳机是否有话筒,inttype=0表示耳机没有话筒,inttype=1表示有话筒</font>

```java
int inttype=intent.getIntExtra("microphone",0);
```

> 我个人的建议就是将一部分代码（根据个人情况而定）放到服务里面，或者是Application里面。

#### 3.监听耳机和外设的代码

```java
public class MainActivity extends Activity {
 //耳机的广播
 public static final String TAGLISTEN = "android.intent.action.HEADSET_PLUG";
 //usb线的广播
 private final static String TAGUSB = "android.hardware.usb.action.USB_STATE";
 //外设的广播
 public static final String TAGIN = "android.hardware.usb.action.USB_DEVICE_ATTACHED";
 public static final String TAGOUT = "android.hardware.usb.action.USB_DEVICE_DETACHED";
  private boolean BOOLEAN=false;
 @Override
 protected void onCreate(Bundle savedInstanceState) {
   super.onCreate(savedInstanceState);
   setContentView(R.layout.activity_main);
     //赛选器
   IntentFilter filter = new IntentFilter();
   //筛选的条件
   filter.addAction(TAGIN);
   filter.addAction(TAGOUT);
   filter.addAction(TAGUSB);
   //注册广播 动态注册
   registerReceiver(receiver, filter);
 }

 /**
  * 创建广播的类
  */
 BroadcastReceiver receiver = new BroadcastReceiver() {
   @Override
   public void onReceive(Context context, Intent intent) {
     String action = intent.getAction();
     //判断外设
     if (action.equals(TAGIN)) {
       ToastUtils.shwotoast(context, "外设已经连接");
       //Toast.makeText(context, "外设已经连接", Toast.LENGTH_SHORT).show();

     }
     if (action.equals(TAGOUT)) {
       if (BOOLEAN) {
         ToastUtils.shwotoast(context,  "外设已经移除");
         //Toast.makeText(context, "外设已经移除", Toast.LENGTH_SHORT).show();
       }
     }
     //判断存储usb
     if (action.equals(TAGUSB)) {
       boolean connected = intent.getExtras().getBoolean("connected");
       if (connected) {
         ToastUtils.shwotoast(context, "USB 已经连接");
         //Toast.makeText(MainActivity.this, "USB 已经连接",Toast.LENGTH_SHORT).show();

       } else {
         if (BOOLEAN) {
           ToastUtils.shwotoast(context, "USB 断开");
           //Toast.makeText(MainActivity.this, "USB 断开",Toast.LENGTH_SHORT).show();
         }

       }
     }
     //判断耳机
     if (action.equals(TAGLISTEN)) {
       int intExtra = intent.getIntExtra("state", 0);
       // state --- 0代表拔出，1代表插入
       // name--- 字符串，代表headset的类型。
       // microphone -- 1代表这个headset有麦克风，0则没有
       // int i=intent.getIntExtra("",0);
       if (intExtra == 0) {
         if (BOOLEAN) {
           ToastUtils.shwotoast(context,"拔出耳机");
           //Toast.makeText(context, "拔出耳机", Toast.LENGTH_SHORT).show();
         }
       }
       if (intExtra == 1) {
         ToastUtils.shwotoast(context, "耳机插入");
         //Toast.makeText(context, "耳机插入", Toast.LENGTH_SHORT).show();
         int intType = intent.getIntExtra("microphone", 0);
         if (intType == 0) {
           ToastUtils.shwotoast(context, "没有麦克风");
           //Toast.makeText(context, "没有麦克风" + intType,Toast.LENGTH_SHORT).show();
         }
         if (intType == 1) {
           ToastUtils.shwotoast(context,"有话筒" );
           //Toast.makeText(context, "有话筒" + intType,Toast.LENGTH_SHORT).show();
         }
       }

     }
     BOOLEAN=true;
   }

 };
 /**
  * 注销广播
  */
 protected void onDestroy() {
   unregisterReceiver(receiver);
 };
}
```

> <font color = "red"> ToastUtils工具类

```java
public class ToastUtils {
 public static Toast toast=null;
 private ToastUtils toastUtils=new ToastUtils();
 private ToastUtils(){}
   public static void shwotoast(Context context,String msg){
     if (toast==null) {
     toast=Toast.makeText(context, msg, Toast.LENGTH_SHORT);
   }else {
     if (toast!=null) {
       toast.setText(msg);
     }
   }
     toast.show();
 }
}
```

> 下面的一个就是获取每一个Id的端口号通过在Usb的广播里面调用这个方法判断是否是自己的设备，
这样就可完成自己想要的操作了（注意当看到设备的ID是以0x开头的是十六位的 然后转化成十进制的数就能看到自己的东西了）

```java
public class MainActivity extends ActionBarActivity {

   protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
       setContentView(R.layout.activity_main);
       UsbManager usbManager = (UsbManager) getSystemService(Context.USB_SERVICE);
       HashMap<String, UsbDevice> map = usbManager.getDeviceList();
       System.out.println("......................befor....................................");
       for(UsbDevice device : map.values()){
          System.out.println(".......one..........dName: " + device.getDeviceName());
          System.out.println(".......tow.........vid: " + device.getVendorId() + "\t pid: " + device.getProductId());
       }
       System.out.println("........................after..................................");

   }
}
```

> <font color = "red">结果如下图所示

![结果]({{ '/styles/images/adroid-external-device/1.png' | prepend: site.baseurl  }})
