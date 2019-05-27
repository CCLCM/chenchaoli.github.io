---
layout: post
title:  Java 网络编程
date:   2018-05-25 01:08:00 +0800
categories: Java
tag: Java 网络编程
---

* content
{:toc}

网络编程：
端口：
物理端口：
逻辑端口：用于标识进程的逻辑地址，不同进程的标识；有效端口：0~65535，其中0~1024系统使用或保留端口。

java 中ip对象：InetAddress.

```Java

import java.net.*;
class  IPDemo{
	public static void main(String[] args) throws UnknownHostException{
		//通过名称(ip字符串or主机名)来获取一个ip对象。
		InetAddress ip = InetAddress.getByName("www.baidu.com");//java.net.UnknownHostException
		System.out.println("addr:"+ip.getHostAddress());
		System.out.println("name:"+ip.getHostName());
	}
}

```


Socket：★★★★，套接字，通信的端点。
就是为网络服务提供的一种机制，通信的两端都有Socket，网络通信其实就是Socket间的通信，数据在两个Socket间通过IO传输。

UDP传输：
1，只要是网络传输，必须有socket 。
2，数据一定要封装到数据包中，数据包中包括目的地址、端口、数据等信息。

直接操作udp不可能，对于java语言应该将udp封装成对象，易于我们的使用，这个对象就是DatagramSocket. 封装了udp传输协议的socket对象。

因为数据包中包含的信息较多，为了操作这些信息方便，也一样会将其封装成对象。这个数据包对象就是：DatagramPacket.通过这个对象中的方法，就可以获取到数据包中的各种信息。



DatagramSocket具备发送和接受功能，在进行udp传输时，需要明确一个是发送端，一个是接收端。



udp的发送端：
1，建立udp的socket服务，创建对象时如果没有明确端口，系统会自动分配一个未被使用的端口。
2，明确要发送的具体数据。
3，将数据封装成了数据包。
4，用socket服务的send方法将数据包发送出去。
5，关闭资源。



```Java
import java.net.*;
class  UdpSend{
	public static void main(String[] args)throws Exception {
//		1，建立udp的socket服务。
		DatagramSocket ds = new DatagramSocket(8888);//指定发送端口，不指定系统会随机分配。
//		2，明确要发送的具体数据。
		String text = "udp传输演示 哥们来了";
		byte[] buf = text.getBytes();
//		3，将数据封装成了数据包。
		DatagramPacket dp = new DatagramPacket(buf,
buf.length,InetAddress.getByName("10.1.31.127"),10000);
//		4，用socket服务的send方法将数据包发送出去。
		ds.send(dp);
//		5，关闭资源。
		ds.close();
	}
}
```


udp的接收端：
1，创建udp的socket服务，必须要明确一个端口，作用在于，只有发送到这个端口的数据才是这个接收端可以处理的数据。
2，定义数据包，用于存储接收到数据。
3，通过socket服务的接收方法将收到的数据存储到数据包中。
4，通过数据包的方法获取数据包中的具体数据内容，比如ip、端口、数据等等。
5，关闭资源。



```Java

class UdpRece {
	public static void main(String[] args) throws Exception{
//		1，创建udp的socket服务。
		DatagramSocket ds = new DatagramSocket(10000);
//		2，定义数据包，用于存储接收到数据。先定义字节数组，数据包会把数据存储到字节数组中。
		byte[] buf = new byte[1024];
		DatagramPacket dp = new DatagramPacket(buf,buf.length);
//		3，通过socket服务的接收方法将收到的数据存储到数据包中。
		ds.receive(dp);//该方法是阻塞式方法。
//		4，通过数据包的方法获取数据包中的具体数据内容，比如ip，端口，数据等等。
		String ip = dp.getAddress().getHostAddress();
		int port = dp.getPort();
		String text = new String(dp.getData(),0,dp.getLength());//将字节数组中的有效部分转成字符串。
		System.out.println(ip+":"+port+"--"+text);
//		5，关闭资源。
		ds.close();
	}
}

```

TCP传输：两个端点的建立连接后会有一个传输数据的通道，这通道称为流，而且是建立在网络基础上的流，称之为socket流。该流中既有读取，也有写入。

tcp的两个端点：一个是客户端，一个是服务端。
客户端：对应的对象，Socket
服务端：对应的对象，ServerSocket

TCP客户端：
1，建立tcp的socket服务，最好明确具体的地址和端口。这个对象在创建时，就已经可以对指定ip和端口进行连接(三次握手)。
2，如果连接成功，就意味着通道建立了，socket流就已经产生了。只要获取到socket流中的读取流和写入流即可，只要通过getInputStream和getOutputStream就可以获取两个流对象。
3，关闭资源。

```Java

import java.net.*;
import java.io.*;
//需求：客户端给服务器端发送一个数据。
class  TcpClient{
	public static void main(String[] args) throws Exception{
		Socket s = new Socket("10.1.31.69",10002);
		OutputStream out = s.getOutputStream();//获取了socket流中的输出流对象。
		out.write("tcp演示，哥们又来了!".getBytes());
		s.close();
	}
}

```

TCP服务端：
1，创建服务端socket服务，并监听一个端口。
2，服务端为了给客户端提供服务，获取客户端的内容，可以通过accept方法获取连接过来的客户端对象。
3，可以通过获取到的socket对象中的socket流和具体的客户端进行通讯。
4，如果通讯结束，关闭资源。注意：要先关客户端，再关服务端。


```Java

class  TcpServer{
	public static void main(String[] args) throws Exception{
		ServerSocket ss = new ServerSocket(10002);//建立服务端的socket服务
		Socket s = ss.accept();//获取客户端对象
		String ip = s.getInetAddress().getHostAddress();
		System.out.println(ip+".....connected");
//		可以通过获取到的socket对象中的socket流和具体的客户端进行通讯。
		InputStream in = s.getInputStream();//读取客户端的数据，使用客户端对象的socket读取流
		byte[] buf = new byte[1024];
		int len = in.read(buf);
		String text = new String(buf,0,len);
		System.out.println(text);
//		如果通讯结束，关闭资源。注意：要先关客户端，在关服务端。
		s.close();
		ss.close();
	}
}
```
