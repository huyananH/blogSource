---
title: Java面试题
date: 2017-12-15 09:01:56
tags:
  - Java
  - 面试
---

### 1. 前言

不知道说什么.....

<!-- more -->

### 2. 笔试题

* 1. Java中几种数据类型，各占多少字节
  * a) 基本数据类型
    整型：short（1）、byte（2）、int（4）、long（8）
    字符型：char（2）
    浮点型：float（4）、double（8）
    布尔型：boolean
  * b) 引用数据类型
    数组
    类（class）
    接口（interface）

* 2. String 能被继承吗？为什么？
不可以。String是final，final类型的数据不可以被继承，实现细节不允许改变。

* 3. String、StringBuffer、StringBuilder的区别
  * a) 三者在执行速度方面的比较：StringBuilder > StringBuffer > String
  * b) String < (StringBuffer、StirngBuilder)的原因
    String 字符串常量
    StringBuffer 字符串变量
    StringBuilder 字符串变量

    上边说String是字符串常量，那么对于以下代码会有疑惑
    ```
    String s = "abc";
    s = s + 1;
    System.out.println(s); //输出abc1
    ```
    明明改变了s字符串，为什么我们说没有改变呢，JVM是这样解析这段代码的。
    先创建一个s对象，赋值abc。然后再创建一个s对象，用来执行第二行代码。所以我们没有改变第一行的s对象。由于这种机制，所以我们每次操作字符串时，都在不断的创建对象，之前的对象被GC回收。

    而StringBuilder和StringBuffer就不一样了，他们是可变的，所以不会一直创建对象，速度肯定比String快。
    特例：
    ```
    String s = "a" + "b" + "c";
    StringBuilder builder = new StringBuilder("a").append("b").append("c");
    ```
    这个特例是String的执行速度比StringBuilder快。是因为JVM
    String s = "abc";这样执行的，所以速度肯定快。
    注意：如果你的字符串来自于String对象，那速度就很慢了。
  * c) 线程安全方面
    String对象不可变，可以理解为常量，显然是    线程安全
    StringBuffer对方法或调用方法加了同步锁     线程安全
    StirngBuilder  没有对方法加同步锁        线程不安全

  * 总结：
      String        操作少量的数据
      StringBuilder 单线程操作字符串缓冲区下的大量数据
      StringBuffer  多线程操作字符串缓冲区下的大量数据

* 4. 类的实例化顺序，比如：父类静态数据、构造方法、字段、子类静态数据、构造方法、字段，当new的时候，他们的执行顺序
  * a) 父类静态变量
  * b) 父类静态代码块
  * c) 子类静态变量
  * d) 子类静态代码块
  * e) 父类非静态变量（父类实例成员变量）
  * f) 父类构造方法
  * g) 子类非静态变量（子类实例成员变量）
  * h) 子类构造方法

* error和Exception的区别
error是

* String 是基本数据类型吗？
不是，String类是final类型的，所以不能继承，不可修改。为了提高效率节省空间，可以使用StringBuilder。

* OSI七层模型
  * 物理层
  * 数据链路层
  * 网络层
  * 传输层
  * 会话层
  * 表示层
  * 应用层

### 3. 面试题

#### 1. 排序

* 冒泡排序
```
public static void sortBubbling (int[] a) {
  int swap = 0;
  for (int i=0; i<a.length; i++) {
    for (int j=i; j<a.length; j++) {
      if (a[j] > a[i]) {
        swap = a[i];
        a[i] = a[j];
        a[j] = swap;
      }
    }
  }
  System.out.print(Array.toString(a));
}
```

* 二分法排序
tag是要查找的数值，返回它的下标
前提：这个数组是有序的
```
public static int sortMiddle(int a[], tag) {
  int first = 0;
  int end = a.length;
  for(int i=0; i<a.length; i++){
    int middle = (first + end)/2;

    if (tag = a[middle]) {
      return middle;
    }

    if (tag > a[middle]){
      first = first + 1;
    }

    if (tag < a[middle]) {
      end = end - 1;
    }
  }
  return 0;
}
```

#### 2. 对ajax的理解

先了解一下同步和异步

同步：发送方发送数据后，等接收方发回响应以后才发下一个数据包的通讯方式。
异步：发送方发送数据后，不等接收方发回响应，接着发下一个数据包的通讯方式。

ajax属于异步请求，即局部刷新。用户体验感强。

#### 3. 父类与子类的调用顺序

a) 父类静态代码块
b) 子类静态代码块
c) 父类构造方法
d) 子类构造方法
e) 子类普通方法
f) 重写父类的方法，则打印重启后的方法

#### 4. 内部类和外部类的调用

a) 内部类可以直接调用外部类包括其private成员变量。使用外部类引用时，用this.关键字调用即可。
b) 外部类调用内部类时，需要建立内部类对象。
