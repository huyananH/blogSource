---
title: Spring Boot基础日志配置
date: 2018-01-17 16:41:04
tags:
  - Spring Boot
---

<img src="/assets/postImg/springbootlogsLogo.jpeg" width="350px" height="350px">

### 一、 前言

今天朋友给推荐了一首歌，然后，不在一个频道的对话就开始了......
> DW：听了好几天
DW：一百万个可能
DW：very 好听
DW：特别能形容你现在的心情，隔着屏幕都能感觉到悲伤

然后......

> 我：这歌，歌词很少，还没翻译......听不懂啊
DW：你听对了吗
我：此处我发了一张截图，此处是一首叫 very 的歌
DW：......

哈哈哈哈哈，隔着屏幕有没有感觉到我的大笑......

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=498139314&auto=1&height=66"></iframe>

这个歌挺好听，但是版权原因只能放这个了......
可以自己去搜听......

<!-- more -->

Spring Boot在所有内部日志中使用Commons Logging，但是默认配置也提供了对常用日志的支持，如：Java Util Logging，Log4J, Log4J2和Logback。每种Logger都可以通过配置使用控制台或者文件输出日志内容。

### 二、 Spring Boot 默认日志

SLF4J——Simple Logging Facade For Java，它是一个针对于各类Java日志框架的统一Facade抽象。Java日志框架众多——常用的有java.util.logging, log4j, logback，commons-logging, Spring框架使用的是Jakarta Commons Logging API (JCL)。而SLF4J定义了统一的日志抽象接口，而真正的日志实现则是在运行时决定的——它提供了各类日志框架的binding。

Logback是log4j框架的作者开发的新一代日志框架，它效率更高、能够适应诸多的运行环境，同时天然支持SLF4J。

默认情况下，Spring Boot会用Logback来记录日志，并用INFO级别输出到控制台。在运行应用程序和其他例子时，你应该已经看到很多INFO级别的日志了。
![日志图片](/assets/postImg/springdefaultlogs.jpg)

从上图可以看到，日志输出内容元素具体如下：

* 时间日期：精确到毫秒
* 日志级别：ERROR, WARN, INFO, DEBUG or TRACE
* 进程ID
* 分隔符：--- 标识实际日志的开始
* 线程名：方括号括起来（可能会截断控制台输出）
* Logger名：通常使用源代码的类名
* 日志内容

实际开发中我们不需要直接添加该依赖，你会发现spring-boot-starter其中包含了 spring-boot-starter-logging，该依赖内容就是 Spring Boot 默认的日志框架 logback。
