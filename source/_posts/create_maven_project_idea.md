---
layout: post
title: IDEA 新建一个基于Maven的Spring Boot项目
date: 2017-09-20 09:32:53
comments: true
toc: true
tags:
      - IDEA
      - Maven
      - Spring Boot
reward: true
---

<p><img src="/assets/ideaCreateMavenP/titlePic.png" width="350px" height="350px"></p>

### 1. 前言

在介绍创建Maven项目前，我还是特别想分享一下，今天遇到的一个特别懵*的事情......

今天，我写了一个登录的API，然后前台进行调用，我们可爱的前台，获取用户信息的时候，居然用的是她从前台接收的用户信息......

<!-- more -->

我和朋友是这么比喻前台的这种做法.....就好比吃饭能吃饱，那吃鞋也照样能吃饱......

我认知的世界是不是有问题......

给我一个擦汗的表情.....

### 2. 新建一个项目
File->New->File
<img src="/assets/ideaCreateMavenP/newFile.png" width="600px"; height="400px">

### 3. 使用Spring Initializer页面工具来创建Spring boot项目
<img src="/assets/ideaCreateMavenP/useSprIniti.png" width="600px"; height="400px">

### 4. 下图所示的工程信息窗口，在这里我们可以编辑我们想要的工程信息
Group：Java包的结构
Artifact：项目的唯一标识符，实际对应项目的名称，就是项目根目录的名称
Type：项目类型
Language：语言
Packaging：编译后的文件类型
Java Version：Java版本
Description：项目描述
Package：包名
<img src="/assets/ideaCreateMavenP/projectInfo.png" width="600px"; height="400px">


### 5. Web->Web （如果使用MongoDB）Web->Web SQL->JPA（如果使用MySQL）
<img src="/assets/ideaCreateMavenP/choiceWebSql.png" width="600px"; height="400px">

### 6. 选择项目路径
<img src="/assets/ideaCreateMavenP/choiceProjPath.png" width="600px"; height="400px">

点击finish完成

### 7. 进入工作空间

![生成之后的项目结构](/assets/postImg/enterProjectIDEA.jpg)

删除.mvn mvnw mvnw.cmd
![删除后的目录文件](/assets/postImg/deleteFileProjectIDEA.jpg)

pom.xml添加tomcat依赖、设置jdk版本、设置Spring版本
```
<!-- 添加tomcat依赖 -->

<dependencies>
  <dependency>
  	<groupId>org.springframework.boot</groupId>
  	<artifactId>spring-boot-starter-tomcat</artifactId>
  	<version>${spring.version}</version>
  </dependency>
</dependencies>

<!-- 设置jdk版本 -->
<!-- 设置spring版本 -->
<properties>
  <project.build.jdk>1.8</project.build.jdk>
  <spring.version>1.5.9.RELEASE</spring.version>
</properties>
```

pom.xml设置maven编译插件、测试插件surefire
```
<build>
  <plugins>
    <!-- 设置maven编译插件 -->
    <!-- mvn install 默认使用的是javac1.3版本，使用以下方法指定版本 -->
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <configuration>
        <source>${project.build.jdk}</source>
        <target>${project.build.jdk}</target>
        <encoding>${project.build.sourceEncoding}</encoding>
      </configuration>
    </plugin>
  </plugins>
</build>
```
