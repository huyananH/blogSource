---
title: Docker的安装以及镜像的获取、查看、搜索、删除、创建
date: 2017-11-08 13:02:24
comments: true
reward: true
tags:
  - Docker
  - Images
---

<img src="/assets/postLog/installDockerLog.jpg" width="350px" height="365px">

### 1. 前言

接下来，我要将的是在MAC本上的操作。

<!-- more -->

### 2. 安装Docker

* *-- 在[Docker官网](https://www.docker.com/) 下载Docker，如图*

![下载Docker的图片教程](/assets/postImg/downloadDocker.jpg)

* 安装Docker，点击启动Docker。

### 3. 从Docker Hub上下载最新的Ubuntu镜像

* 在命令行输入：
```
docker pull ubuntu
```
* 完成之后会有如下提示

![ubuntu镜像下载完成](/assets/postImg/pllUbuntuSuccTip.jpg)

* 使用下载的镜像，创建一个容器
```
docker run -it ubuntu /bin/bash
```

* 输入密码之后就会进入到容器中，目录如图：

![Ubuntu的目录结构](/assets/postImg/UbuntuContent.jpg)

### 4. Docker的基本操作

* 查看Docker上已有的镜像
```
docker images
```
