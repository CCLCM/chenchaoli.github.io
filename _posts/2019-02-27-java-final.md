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
     被final修饰的方法,不能在子类中覆盖.

> <font color = "red">因此父类中有被final的方法又想要子类也有同样的方法,那么只需要将父类的final方法变成 private,子类此时的方法不属于复写,而是属于子类自己的方法.</font>

> 如下代码,public final的方法不允许被复写,报cannot override 'getNumber()' , overridden method is final 错误

```java
public class TestClass {
    public TestClass(){

    }

    public final void getNumber(){

    }

}

class A extends TestClass{
    //getNumber()' cannot override 'getNumber()' , overridden method is final
    public void getNumber(){

    }

}

```

> 如下代码将 getNumber()方法 由public 变成private, 编译通过,Class A 中的getNumber() 方法属于Class A

```java
public class TestClass {
    public TestClass(){

    }

    private final void getNumber(){

    }

}

class A extends TestClass{
    //getNumber()' cannot override 'getNumber()' , overridden method is final
    public void getNumber(){

    }

}
```

#### 3.final修饰变量
     被final修饰的成员变量只能被赋值一次,赋值后,不允许改变,只能为读取类型,赋值可以在声明的时候赋值,也可以在所有构造很熟内赋值不报错.

> <font color = "red">原因是被final修饰的变量,引用的地址值不能发生变化,但是当被fina修饰一个引用类的时候,不允许该引用类指向其他的对象,但是该引用所指向的对象的内容是可以变化的</font>

> 如下代码, mNum 二次赋值,被会报错,而声明的时候不赋值, 所有构造很熟内赋值不报错.

```java
public class TestClass {
    private final int mNum = 666;  
    public TestClass(){
        //不可被重新赋值
        //Cannot assign a value to final variable 'mNum'
        mNum = 6688;
    }

}
```
>在声明的时候不赋值

```java
public class TestClass {
    private final int mNum;
    public TestClass(){
        //不会报错
        mNum = 6688;
    }

}
```

> fina修饰一个引用类 obj 时候,obj不允许该引用类指向其他的对象 ,报错 Cannot assign a value to final variable 'obj',但是 obj所指向的对象,也就是new Object() 中的内容是可以改变的

```java
public class TestClass {

    public TestClass() {
     final Object obj = new Object();
     // Cannot assign a value to final variable 'obj'
     obj = new Object();
    }

}
```

#### 后续持续更新







































<!-- https://www.cnblogs.com/xiaoxi/p/6392154.html -->
