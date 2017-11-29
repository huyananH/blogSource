---
title: 开始学习Node.js
date: 2017-11-28 13:21:54
tags:
  - Node.js
---

<img src="/assets/postLog/nodeFirstLog.jpeg" width="350px" height="350px">

### 1. 前言

昨晚两点才睡，所以打算学点东西让自己清醒一下。

我决定学习Node.js，接下来是万年不变的Hello World!

e...

还是写写吧，不要问我为什么，就是闲。

<!-- more -->

### 2. 万年不变的Hello World！

* 创建一个server.js
* 开始编写程序
```
var http = require("http");

http.createServer(function() {

  //发送HTTP头部
  //HTTP 状态 200 OK
  //HTTP 类型 text/plain
  response.writeHead(200,{"Content-type" : "text/plain"});

  //响应内容
  response.end("Hello World");
  }).listen("8888");

  console.log("Server run http://localhost:8888");
```

运行结果：
  * 终端：
  ![终端输出](/assets/postImg/node_first_console.jpg)
  * 浏览器：
  ![浏览器输出](/assets/postImg/node_first_success.jpg)
