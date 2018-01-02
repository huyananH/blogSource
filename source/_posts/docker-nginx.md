---
title: Nginx 静态服务器
date: 2017-12-26 12:56:59
tags:
  - Nginx
---

### 1. 下载Nginx源码

*-- [Nginx下载地址](https://nginx.org/en/download.html)*，下载Nginx源码，一般分为开发版和稳定版。
<!-- more -->
我下载的是红线勾的稳定版
![Nginx下载界面](/assets/postImg/nginxDownload.jpg)

### 2. 安装

* 安装zlib
* 安装openssl
* 安装

* 安装nginx

./configure --prefix=/usr/local/nginx \
--sbin-path=/usr/local/nginx/nginx \
--conf-path=/usr/local/nginx/nginx.conf \
--pid-path=/usr/local/nginx/nginx.pid \
--with-pcre=/usr/local/src/pcre-8.40 \
--with-zlib=/usr/local/src/zlib-1.2.11 \
--with-openssl=/usr/local/src/openssl \
--with-http_ssl_module \
--add-module=/usr/local/src/nginx_upload_module-2.2.0

### 3. 参数解释
* 默认安装
```
./configure
```

* 自定义安装
（设置程序文件目录）Nginx可执行文件的安装路径，只能安装时指定，如果没有指定，则默认设置为<prefix>/sbin/nginx
./configure --sbin-path=<path>

（设置配置文件（nginx.conf）目录）设置在nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的-c选项。默认为prefix/conf/nginx.conf
--conf-path=<path>

 （设定pid文件(nginx.pid)目录）将存储的主进程的进程号。安装完成后，可以随时改变的文件名 ， 在nginx.conf配置文件中使用 PID指令。默认情况下，文件名 为prefix/logs/nginx.pid.
--pid-path=<path>

 使用https协议模块。默认情况下，该模块没有被构建。建立并运行此模块的OpenSSL库是必需的。
--with-http_ssl_module

设置PCRE库的源码路径。PCRE库的源码（版本4.4 - 8.30）需要从PCRE网站下载并解压。其余的工作是Nginx的./ configure和make来完成。正则表达式使用在location指令和 ngx_http_rewrite_module 模块中。
--with-pcre=/opt/app/openet/oetal1/chenhe/pcre-8.37 \

设置的zlib库的源码路径。要下载从 zlib（版本1.1.3 - 1.2.5）的并解压。其余的工作是Nginx的./ configure和make完成。ngx_http_gzip_module模块需要使用zlib 。
数据压缩用的函式库
--with-zlib=/opt/app/openet/oetal1/chenhe/zlib-1.2.8 \

设置openssl的路径
--with-openssl=/opt/app/openet/oetal1/chenhe/openssl-1.0.1t
