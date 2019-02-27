---
layout: post
title:  java语法中的final关键字
date:   2019-02-26 00:00:00 +0800
categories: Java
tag: java语法中的final关键字
---

* content
{:toc}


final关键字			{#final}
====================================

今天看了Java编程思想,看到final关键字,回想了一下final关键字的用法,在此做笔记保留方便后期复习用.

Java中final关键字可以用来修饰 <font color = "red">类</font> ,<font color = "red">方法</font>,<font color = "red">成员变量</font>,<font color = "red">局部变量</font> 接下来咱们挨个描述一下.
<br/>
#### 1.首先是修饰类
     final的中文意思是 最后,最终.也就是说不能改变的,被final修饰的类有以下几个特点

> <font color = "red">类被final修饰的时候,这个类不能被继承,要注意的是final内中的所有成员和类中的方法都会被隐式的指定为final方法</font>

> 代码以AndroidStudioIDE 编写.

> 如下代码

```java
final class TestClass {
    public TestClass(){

    }

}
// Cannot inherit from final 报此类错误
public class A extends TestClass{

}
```

> <font color = "red">一般的开发过程中 类很少设置为final,除非有特殊要求,没有特殊要求建议不要设置成final,不利于后期的拓展 </font>


#### 2.final修饰类中的方法
<!-- https://www.cnblogs.com/xiaoxi/p/6392154.html -->
