---
layout: post
title:  简单工程模式
date:   2017-02-23 00:00:00 +0800
categories: 设计模式
tag: 设计模式,简单工程模式
---

* content
{:toc}



![simple-factory]({{ '/styles/images/design_mode/simple-factory.jpg' | prepend: site.baseurl  }})

<!-- > 是不是还在为了手机usb被占用而不能链接编译器而难过？是不是感觉无线调试遥不可及？
读完下面的几步 让你轻松掌握无线调试。

#### 1. 首先将你的手机连接到无线网<font color = "red">必须和电脑是同一个局域网</font>
<br/>
#### 2. Window 配置好adb Linux 安装好adb





不会配置adb的点击这里 [Window配置adb](https://jingyan.baidu.com/article/17bd8e52f514d985ab2bb800.html) ,[Linux配置adb](https://blog.csdn.net/xiaoma_bk/article/details/80986221)

<br/>
#### 3. 将你的手机用数据线链接到电脑上


#### 4. 打开终端设置tcpip地址

```Ubuntu
$ adb tcpip 5555 （5555为端口号，可以自由指定）
```

#### 5. 查看手机tcpip地址然后在输如下命令

```Ubuntu
$ adb tcpip
```

> 此时你可以查看到 自己手机的ip地址 大概如下所示 10.39.211.183/8 0x000000c1 d2:41:80:1f:55:11

#### 6. 拔掉你的手机最后输入如下代码

```Ubuntu
 $ adb connect手机IP：5555 （如$ adb connect10.39.211.183:5555）
```

>此时你查看你的Android中的 Android Monitor 中已经有设备链接了 <font color = "red"> 此时你可以跑一把自己的程序但是要有心里准备，安装过程可能比较慢</font>。


>也可以使用如下命令查看是否已经链接上

```Ubuntu
 $ adb devices
```

><font color = "red">如果此时你未拔掉USB可以看到链接是两个设备，多个设备只要设置的端口号不同都可以进行链接</font>。

调试完成之后可用如下命令 或着重新启动手机即可

```Ubuntu
$ adb kill-server
```


#### 总结：

> 无线调试的优点：方便、灵活、在有效距离内都是可以使用的，非常适合电视基机顶盒和手机需要外设的开发进行调试
无线调试的缺点：信号受周围环境影响会导致不稳定现象,传输速度较慢，Window容易被断开的原因，搜
狗 QQ 酷狗 暴风 这几个设备会抢占手机手机端口，一般退出即可。 -->
