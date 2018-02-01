---
title: Spring Boot 整合 EhCache
date: 2018-01-29 16:57:56
tags:
  - Spring Boot
---

### 一、 前言

Spring boot默认使用的是SimpleCacheConfiguration，即使用ConcurrentMapCacheManager来实现缓存。但是要切换到其他缓存实现也很简单

### 二、 pom.xml添加依赖

```
<dependency>
	<groupId>net.sf.ehcache</groupId>
	<artifactId>ehcache</artifactId>
</dependency>
```

### 三、 创建ehcache配置文件ehcache.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<ehcache>
    <cache name="tests" maxElementsInMemory="1000"/>
</ehcache>
```

### 四、 
