---
title: Spring Boot项目热部署
date: 2017-12-08 09:54:17
tags:
  - Spring Boot
---

<p><img src="/assets/postImg/springbootCorsLogo.jpeg" width="350px" height="350px"></p>

### 一、 前言

今天看了个笑话，笑话的内容是这样的
> 我们宿舍本来的规定是谁穿的衣服多谁下去拿外卖。
当我全都脱了之后，舍友拍了个照。
威胁我，让我去拿外卖......

我真的不是故意要笑的，真的笑死了，太过分......

哈哈哈

<!-- more -->

在开发项目的时候，经常会修改页面或者是修改及后太代码，为了显示修改后的效果，总需要重启应用，其实在编译新的Class文件，然后被虚拟机的ClassLoader加载。
热部署就是监听到如果有Class文件改动，就会重新编译该Class文件，并创建新的ClassLoader进行加载该文件。提高开发效率。

### 二、 添加依赖

在pom.xml文件中添加依赖 spring-boot-devtools
```
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-devtools</artifactId>  
    <!-- 依赖不会传递 -->
<optional>true</optional>  
```

### 三、 修改配置文件 application.properties
配置监听的文件路径
spring.devtools.restart.additional-paths=

### 四、 重启应用

重启应用之后，再修改，就会自动加载
