---
title: 准备java面试中遇到的细节问题
date: 2017-12-19 14:21:09
tags:
  - Java
---

### 1. 目录
* switch
* 如何防止sql注入
* Mysql的乐观锁与悲观锁
* 写出SQL的左连接、右连接、内连接的关键字
* Linux命令行。有一个日志文件a.log，计算包含"out"的行数；将文件中的"out"替换成"ss"
* 某站点无法访问，可能原因及证明

<!-- more -->

### 2. switch
* switch(Exp)
Exp支持的类型是：byte、short、int、char、enum。在java7之后也支持 字符串 类型的

switch(type) {
  default:
    System.out.println(4);
  case 1:
    System.out.println(1);
}
遇到以下几种情况
* 假如type=4
  * 输出结果为
    4
    1
  * 原因：
    case中没有满足条件的，会默认执行default。
    因为没有break，它会执行满足条件以及之后的语句。

### 3. 如何防止sql注入
* 防止sql注入
  * 使用正则表达式过滤传入的参数
  * 不使用拼装sql语句，例如：select * from user where user_name = '+"user_naem"';
  * 字符串过滤
  * jsp调用该函数检查是否包含非法字符
  * jsp页面判断代码

### 4. Mysql的乐观锁与悲观锁

* 悲观锁（先取锁，再访问）
>
是一种并发控制的方法。它可以阻止一个事务以影响其他用户的方式来修改数据。如果一个事务执行的操作都某行数据应用了锁，那只有当这个事务把锁释放，其他事务才能够执行与该锁冲突的操作。

悲观锁的实现
  * 要使用悲观锁，必须先关闭Mysql自动提交属性。 set autocommit = 0;
  * 开启事务
    * begin;
    * begin work;
    * start transaction;
  * 通过排它锁来实现悲观锁 select ... for update; 来锁定某条或某些数据；
    例如：select * from user where id=1 for update;
    这样，在这个事务未结束之前，其他事务是不能对这条数据进行修改的
  * 提交事务
    * commit;
    * commit work;

备注：如果要验证是否加锁成功，要在不同的命令终端。

* 乐观锁
>
它假设多用户并发的事务在处理时不会彼此互相影响，各事务能够在不产生锁的情况下处理各自影响的那部分数据。在提交数据更新之前，每个事务会先检查在该事务读取数据后，有没有其他事务又修改了该数据。如果其他事务有更新的话，正在提交的事务会进行回滚。

### 5. 写出SQL的左连接、右连接、内连接的关键字
* 左连接 left join on
* 右连接 right join on
* 内连接 inner join on

### 6. Linux命令行。有一个日志文件a.log，计算包含"out"的行数；将文件中的"out"替换成"ss"；

* grep 查询文件里符合条件的行
* wc 用于计算字数
  -l 只显示行数
  -w 只显示字数
  -c 只显示字节数
* sed 处理文本文件

计算包含out的行数
```
cat a.log | grep out | wc -l
```
Or
```
grep out a.log | wc -l
```

将文件中的“out”替换成“ss”
sed 's/要被取代的字符串/新的字符串/g' 文件名
```
sed 's/out/ss/g' a.log
```

### 7. 三次握手和四次挥手

* 三次握手
  * 客户端对服务器说：我要发消息了啊
  * 服务器对客户端说：你发呗，墨迹什么
  * 然后客户端开始发消息

* 四次挥手
  * 客户端对服务器说：你别给我发消息了
  * 服务器对客户端说：不发就不发，真是的
  * 服务器对客户端说：你千万别再给我发消息
  * 客户端对服务器说：看把你美得，我才懒得理你

### 8. Spring 事务管理的方式
* 基于Xml配置
* 基于注解
