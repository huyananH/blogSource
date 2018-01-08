---
title: Spring Boot 跨域配置
date: 2018-01-04 13:41:12
tags:
  - Spring Boot
  - 跨域
---

### 一、前言

前端时间忘总结这一块了，我部署到服务器上的程序，我的搭档访问不到，带我的大神和我们说，配置一下跨域。
刚开始我不理解跨域，只是在网上搜了搜，然后添加好，那边说能访问到了，也就没再管。
这两天又用到了，所以再这总结一下。

<!-- more -->

### 二、对跨域的理解

在网上看到这个，我感觉一看就明白了，所以我放到了这里

跨域，指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器施加的安全限制。

所谓同源是指，域名，协议，端口均相同，不明白没关系，举个栗子：

http://www.123.com/index.html 调用 http://www.123.com/server.php （非跨域）

http://www.123.com/index.html 调用 http://www.456.com/server.php （主域名不同:123/456，跨域）

http://abc.123.com/index.html 调用 http://def.123.com/server.php （子域名不同:abc/def，跨域）

http://www.123.com:8080/index.html 调用 http://www.123.com:8081/server.php （端口不同:8080/8081，跨域）

http://www.123.com/index.html 调用 https://www.123.com/server.php （协议不同:http/https，跨域）

请注意：localhost和127.0.0.1虽然都指向本机，但也属于跨域。

浏览器执行javascript脚本时，会检查这个脚本属于哪个页面，如果不是同源页面，就不会被执行。

### 三、Spring Boot项目，跨域配置

* 在项目中添加文件CorsConfig.java，代码如下

```
package cn.dfusion.demo.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

@Configuration
public class CorsConfig extends WebMvcConfigurerAdapter {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowCredentials(true)
                .allowedMethods("GET", "POST", "DELETE", "PUT")
                .maxAge(3600);
    }

}

```

至此，跨域就配置好了
