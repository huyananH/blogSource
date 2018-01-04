---
title: Postman 
date: 2018-01-04 13:17:49
tags:
  - Postman
---

### 一、前言
我写的是一个前后端分离的系统，今天在调试程序的时候，遇到了一个问题。在写登录拦截之后，会自动检测request的头部信息中，是否存在token，来检测是否让此用户进行操作。但是，用传统的测试根本无法在request中头部信息中添加参数。

为此，我在网上搜了一下，发现了一个特别好用的工具--Postman

<!-- more -->

### 二、Postman安装

* 官网下载
*-- [下载地址](https://www.getpostman.com/app/download/osx64)*

* 下载完打开
如下图，可以为请求添加参数，为request的header中添加参数
![postman界面](/assets/postImg/postman_open.jpg)
