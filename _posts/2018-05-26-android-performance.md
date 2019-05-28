---
layout: post
title:  "Android APP的性能优化"
date:   2018-05-26 16:03:06 +0800
categories: Android
tag: Android APP的性能优化
---

* content
{:toc}

------------------------
><font color= "red">Android 性能优化分为四个方向 ，这四个方向中又可以细分好多，接下来慢慢讲解</font>

1.稳定性。

2.流畅性。

3.低耗能(流量，电能)。

4.安装包大小



> 第一个稳定性,是最基本的要求,首先保证用户在使用的过程中应用不能 闪退,不能崩溃.

影响应用稳定性的包括下面几个方面:

1)ANR问题

   ANR(Application Not Responding)是用户在使用应用的过程中,app出现卡顿之后的二次点击或者
   是BroadCast10s内没有响应,Service20s内没有完成导致的.


2)OOM问题
   OOM(Out Of Memory) 应用消耗大量的内存,导致系统内存使用紧张,从而导致应用被系统杀掉.

3)Crash问题

  Crash 最常见的问题,由空指针,文件资源找到不到等早曾的.

1)

1)

1)
