---
title: 安装Redis
date: 2017-10-23 09:28:21
comments: true
reward: true
tags:
  - Redis
---
<img src="/assets/postLog/installRedisMacLog.jpeg" width="350px" height="350px">

### 一、 前言

在介绍今天主题之前，我先要分享一件比较有意思的事情......

今天，和朋友聊天，朋友让我去看他今天发的朋友圈，朋友圈是这样写的......

<!-- more -->

> 今天出去吃饭，点了一碗粥和一碗馄饨，粥快、馄饨慢，我就先喝粥，等馄饨......

> 粥喝一半，馄饨叫号了，去拿馄饨，回头粥没了，仿佛空气都凝固了......

> 服务员大婶小小的动作和无辜的眼神伤害竟然这么大......

我还是忍不住笑了......

朋友说，没事笑吧，点个赞也没关系的......

然后，我就点了个赞......

在安装Redis之前，先对Redis进行一个简单的了解。

Redis是一个完全免费开源的，遵守BSD协议，是一个高性能的key-value数据库。

解释：BSD协议

BSD开源协议是一个给于使用者很大自由的协议。可以自由的使用，修改源代码，也可以将修改后的代码作为开源或者专有软件再发布。当你发布使用了BSD协议的代码，或者以BSD协议代码为基础做二次开发自己的产品时，需要满足三个条件：
 * 如果再发布的产品中包含源代码，则在源代码中必须带有原来代码中的BSD协议。
 * 如果再发布的只是二进制类库/软件，则需要在类库/软件的文档和版权声明中包含原来代码中的BSD协议。
 * 不可以用开源代码的作者/机构名字和原来产品的名字做市场推广。

BSD代码鼓励代码共享，但需要尊重代码作者的著作权。BSD由于允许使用者修改和重新发布代码，也允许使用或在BSD代码上开发商业软件发布和销 售，因此是对商业集成很友好的协议。很多的公司企业在选用开源产品的时候都首选BSD协议，因为可以完全控制这些第三方的代码，在必要的时候可以修改或者 二次开发。

Redis 与其他 key - value 缓存产品有以下三个特点：
 * Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
 * Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
 * Redis支持数据的备份，即master-slave模式的数据备份。


### 二、在MAC下安装Redis

#### 1. 官网下载

*-- 在Redis下载最新稳定版本[点击这里](https://redis.io/)*

这里的稳定版本是4.0.2

<img src="/assets/postImg/download_redis.jpg" width="400px" height="350px">

#### 2. 把压缩包解压，放入/usr/local

#### 3. 切换到相应的目录下面

```
cd /usr/local/redis-4.0.2
```

#### 4. 编译测试

```
sudo make test
```

#### 5. 编译安装

```
sudo make install
```
![编译安装后的样子](/assets/postImg/make_install_redis.jpg)

#### 6. 启动Redis

```
redis-server
```
![启动redis的样子](/assets/postImg/redis_server.jpg)

#### 7. 配置

1. 在Redis目录下建立bin、etc、db三个文件夹
  * 进入Redis根目录
  ```
  cd /usr/local/redis-4.0.2
  ```
  * 创建文件夹
  ```
  sudo mkdir bin
  sudo mkdir etc
  sudo mkdir db
  ```
2. 把/usr/local/redis-4.0.2/src目录下的mkreleasehdr.sh，redis-benchmark， redis-check-rdb， redis-cli， redis-server拷贝到bin目录

3. 拷贝redis.conf 到/usr/local/redis-4.0.2/etc下

4. 修改redis.conf
```
# 修改为守护模式
daemonize yes
# 设置进程锁文件
pidfile /var/run/redis_6379.pid
# 端口
port 6379
# 客户端超时时间
timeout 300
# 日志级别
loglevel debug
# 日志文件位置
logfile /usr/local/redis-4.0.2/log-redis.log
# 设置数据库的数量，默认为0，可以使用SELECT <dbid>命令在连接上指定数据库id
databases 16
##指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
#save <seconds> <changes>
#Redis默认配置文件中提供了三个条件：
save 900 1
save 300 10
save 60 10000
#指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，
#可以关闭该#选项，但会导致数据库文件变的巨大
rdbcompression yes
#指定本地数据库文件名
dbfilename dump.rdb
#指定本地数据库路径
dir /usr/local/redis-4.0.2/db/
#指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能
#会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有
#的数据会在一段时间内只存在于内存中
appendonly no
#指定更新日志条件，共有3个可选值：
#no：表示等操作系统进行数据缓存同步到磁盘（快）
#always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）
#everysec：表示每秒同步一次（折衷，默认值）
appendfsync everysec
```

#### 8. 启动服务
```
./bin/redis-server etc/redis.conf
```
#### 9. 查看日志
```
tail -f log-redis.log
```
![查看日志](/assets/postImg/redis_log.jpg)

#### 10. 打开redis客户端
```
./bin/redis-cli
```
![打开Redis客户端](/assets/postImg/redis_cli.jpg)
执行Redis命令

### 三、在Ubuntu安装Redis

与MAC本安装的区别
* 4. 编译
```
  make
```
这一步报错，安装make、gcc
```
apt-get install make
apt-get install gcc
```
