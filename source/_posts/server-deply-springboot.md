---
title: 在阿里云的Ubuntu服务器部署SpringBoot程序
date: 2017-11-27 15:08:31
tags:
  - Ubuntu
  - Spring Boot
  - Deploy
---

<img src="/assets/postLog/serverDeploySpringbootLog.jpeg" width="350px" height="350px">

### 1. 前言

最近，尝试在阿里云的服务器上部署了一下SpringBoot程序。

接下来，我讲一下......这个坎坷的过程。
<!-- more -->

### 2. 通过ssh命令进入服务器文件系统

```
ssh root@11.11.11.11 -p22
```
* ssh是一种网络协议，用于计算机之间的加密登录。如果一个用户从本地计算机，使用SSH协议登录另一台远程计算机，我们就可以认为，这种登录是安全的，即使被中途截获，密码也不会泄露。
* ssh对应的端口是22端口。
* 11.11.11.11 服务器的IP
* -p22        进入22端口，如果是22端口，这个参数可以不写。如果修改，端口跟在-p后边就可以了。

### 3. 安装mysql

* 更新apt-get
```
apt-get
```

* 下载mysql
```
apt-get install mysql-server
```
询问是否继续时，输入：Y
在安装过程中，会弹出一个界面要求设置mysql的root密码，这里一定要设置，省的安完之后设置。
<![mysql需要设置root密码](/assets/postImg/mysql_password.png)
再次输入密码：
<![再次输入](/assets/postImg/linux_mysql_password_set.png)

* 检验是否安装成功
执行
```
netstat -tap|grep mysql
```
如果出现以下情况，就是安装成功
<![mysql安装成功](/assets/postImg/linux_mysql_success.jpg)

* 查看mysql版本
```
mysql -V
```
结果为：
<![查看mysql版本](/assets/postImg/linux_mysql_version.jpg)

### 4. 把sql文件导入
* 使用FileZilla把sql文件放入服务器文件中，假设放到 /usr/data/video.sql目录下。
* 在shell中进入mysql
```
mysql -uroot -p
```
* 导入sql文件
```
source /usr/data/video.sql
```

### 5. 安装maven

因为我要部署的项目是maven项目，所以需要安装maven
```
apt install maven
```

### 5. 卸载Ubuntu自带的OpenJdk
```
apt-get remove openjdk*
```

### 6. 安装JDK

* 检测服务器是否有JDK
```
java -version
```
如果出现以下结果，则说明JDK已经存在：
<![JDK存在](/assets/postImg/linux_jdk_success.jpg)

如果不存在，接下来安装JDK
* 在官网下载JDK，放入/home/software

* 创建/usr/lib/jvm，以便放入解压后的文件
```
cd /usr/lib
mkdir jvm
```

* 解压并把解压后的文件放入/usr/lib/jvm目录中
```
cd /home/software
tar -xzvf jdk-8u152-linux-x64.tar.gz
mv jdk1.8.0_152 /usr/lib/jvm/
```

* 进入/usr/lib/jvm，重命名java1.8
```
cd /usr/lib/jvm
mv jdk1.8.0_152 java1.8
```

* 添加路径
```
vim /etc/profile
```
按i
添加路径
```
export JAVA_HOME=/usr/lib/jvm/java1.8
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

* 执行命令
```
source /etc/profile
```

* 测试JDK是否安装成功
```
java -version
```
结果是以下结果，则安装成功。
<![jdk安装成功](/assets/postImg/linux_jdk_success.png)

### 7. 直接用springboot内置Tomcat部署
在项目根目录下，执行
```
mvn spring-boot:run -Pdev
```
参数： -P 选择运行环境
