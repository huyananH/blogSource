---
title: 解决Redis之MISCONF Redis is configured to save RDB snapshots, but is currently not able to persist on disk. Commands that may modify the data set are disabled. Please check Redis logs for details about the error.
date: 2017-10-23 16:03:24
comments: true
reward: true
tags:
  -Redis
---

### 一、前言

今天SpringBoot整合Redis，却报标题所示的错误

<!-- more -->

### 二、Redis问题

Redis配置为保存RDB快照，但目前无法在磁盘上持久化。可能修改数据集的命令是禁用的。有关错误的详细信息，请查看Redis日志。

### 三、原因

强制关闭Redis快照，导致不能持久化

### 四、解决方案

将stop-writes-on-bgsave-error设置为no(进入Redis文件夹，打开Redis客户端)
```
cd /usr/local/redis-4.0.2
./bin/redis-cli
127.0.0.1:6379> config set stop-writes-on-bgsave-error no
```
