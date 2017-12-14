---
layout: post
title: Spring Boot多环境配置
date: 2017-10-13 12:27:49
comments: true
toc: true
tags:
    - Spring Boot
    - 多环境切换
reward: true
---

<img src="/assets/mulEnv/titlePic.jpeg" width="350px"; height="350px">

### 1. 前言

从前，我想象中的样子就和上边这张图一样......

<!-- more -->

气质呀......

但是，现实真的很残酷......

女屌丝呀......


### 2. 在src/main/目录下面创建不同环境的文件
> 文件目录
```
    src
        main
            filters
                application-prod.properties
                application-dev.properties
                application-test.properties
```

### 3. pom.xml中添加不同环境
>   
```
        <!--多环境配置-->
    	<profiles>
    		<!--生产环境-->
    		<profile>
    			<id>prod</id>
    			<properties>
    				<env>prod</env>
    			</properties>
    			<!--默认使用该环境-->
    			<activation>
    				<activeByDefault>true</activeByDefault>
    			</activation>
    		</profile>
    		<!--开发环境-->
    		<profile>
    			<id>dev</id>
    			<properties>
    				<env>dev</env>
    			</properties>
    		</profile>
    		<!--测试环境-->
    		<profile>
    			<id>test</id>
    			<properties>
    				<env>test</env>
    			</properties>
    		</profile>
    	</profiles>
```

### 4. 使用Maven的Resource和filter实现多环境切换
> 在build 标签中添加以下代码
>
```
        <filters>
        		<filter>src/main/filters/application-${env}.properties</filter>
        </filters>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
            </resource>
        </resources>
```

### 5. 在src/main/resources 目录下边的application.properties中修改选用的配置文件
```
    spring.profiles.active=@env@
```

### 6. 在filters目录下的文件设置环境变量时，需要在src/main/resources/application.properties文件进行引用
例如：
    application-dev.properties文件

```
server.port=8080
```

application.properties中引用
```
server.port=@server.port@
```
