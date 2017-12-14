---
title: Spring Boot项目热部署
date: 2017-12-08 09:54:17
tags:
  - Java
  - Spring Boot
---

### 1. 前言

在开发项目的时候，经常会修改页面或者是修改及后太代码，为了显示修改后的效果，总需要重启应用，其实在编译新的Class文件，然后被虚拟机的ClassLoader加载。
热部署就是监听到如果有Class文件改动，就会重新编译该Class文件，并创建新的ClassLoader进行加载该文件。提高开发效率。

<!-- more -->

### 2. 添加依赖

在pom.xml文件中添加依赖 spring-boot-devtools
```
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-devtools</artifactId>  
    <!-- 依赖不会传递 -->
<optional>true</optional>  
```

### 3. 修改配置文件 application.properties
配置监听的文件路径
spring.devtools.restart.additional-paths=

### 4. 重启应用

重启应用之后，再修改，就会自动加载
