---
title: Ubuntu目录结构
date: 2017-11-27 12:38:17
tags:
  - Ubuntu
---

<img src="/assets/postLog/ubuntuLog.jpg" width="350px" height="350px">

### 1. 前言

ubuntu基于linux的免费开源桌面PC操作系统，十分契合英特尔的超极本定位，支持x86、64位和ppc架构。

接下来，我总结一下Ubuntu的目录结构。
<!-- more -->

### 2. 目录结构
* /：Linux文件系统的根目录

* /etc：存放文件管理配置文件和目录（系统文件和大部分应用程序的全局配置文件）。
  * /etc/X11：     图形界面配置
  * /etc/profile： “login shell”代表用户登录，例如：使用“su-”命令、或者是“ssh”连接到某一个服务器上，都会使用该用户默认shell启动login shell模式。
  该模式下的shell会自动执行/etc/profile和~/.profile文件，但不会执行任何bashrc文件，所以一般在/etc/profile或者~/.profile里，我们都会手动去soure bashrc文件。
  而 no-login shell的情况是我们在终端下直接输入bash或者bash -c "CMD"来启动shell，该模式下是不会自动去运行任何profile文件。
