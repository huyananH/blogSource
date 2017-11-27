---
title: 在Docker上使用Docker Compose部署Spring Boot项目
date: 2017-11-24 14:11:00
tags:
  - Docker
  - Spring Boot
---

<img src="/assets/postLog/dockerLog.jpg" width="350px" height="350px">

### 1. 前言

接下来，总结我使用Docker部署项目的过程。

<!-- more -->

### 2. 创建一个Spring Boot项目

*-- 前边 [在idea新建一个Maven项目](https://huyananh.github.io/2017/09/20/create_maven_project_idea/)总结过*

* 配置多环境切换，*-- 在[Springboot多环境配置](https://huyananh.github.io/2017/10/13/springboot_mul_env/)总结过*

### 3. docker创建数据库

* 创建mysql容器
```
docker run --name mysqldata  -p 3310:3306 -e MYSQL_ROOT_PASSWORD=123456  mysql:latest
```

* 导入数据库
```
source /Users/eh/video.sql
```
参数：/Users/eh/video.sql --.sql文件的路径

### 4. 项目连接数据库
```
spring.datasource.url=jdbc:mysql://192.168.1.111:3307/video_cjk?useUnicode=true&characterEncoding=utf-8
```
参数：192.168.1.111      --服务器的ip
     3307               --服务器中数据库对应的端口映射
     video_cjk          --数据库名称
     characterEncoding  --编码方式UTF-8

### 5. 编写Dockerfile文件

* 在 src/main 下边建 docker 文件夹。
* 在 src/main/docker 下创建Dockerfile 文件。
* 编写 Dockerfile 文件。
```
FROM frolvlad/alpine-oraclejdk8:slim
VOLUME /tmp
ADD target/video-0.0.1-SNAPSHOT.jar app.jar
RUN sh -c 'touch /app.jar'
ENV JAVA_OPTS=""
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar" ]
```
详解：
     1. FROM

     ```
     FROM <image> [AS <name>]
     ```
     Or
     ```
     FROM <image>[:<tag>] [AS <name>]
     ```
     Or
     ```
     FROM <image>[@<digest>] [AS <name>]
     ```
     "FROM"指令将初始化一个新的编译阶段，并未后续指令设置基础镜像。因此，有效的Dockerfile必须以"FROM"指令开始。该镜像可以是任何有效的镜像，从公共存储库中拉取镜像特别容易。

     * “ARG”是在Dockerfile中唯一一条可以再“FROM”之前的指令。请参考 *-- [了解ARG和FROM如何交互](https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact)*。

     * "FROM"在单个Dockerfile中可以出现多次，去创建多个镜像或者用一个构建阶段作为另一个的依赖项。在每条“FROM”指令之前，只需记下提交输出的最后一个镜像ID。每条“FROM”指令都会清楚以前创建的任何状态。

     * 可选地，通过添加“AS name”到FROM指令，可以给构建阶段起名字。名称可以在以后的使用FROM 和 COPY --from=<name|index>指令去指向镜像构建的这个阶段。

     * “tag”或“digest”值是可选的。如果你忽略他们其中的一个，则把“latest”作为默认。如果找不到“tag”的值，构建则返回错误。

     理解ARG和FROM是如何交互的

     “FROM”指令支持任何在第一个“FROM”之前用“ARG”指令声明的变量。
     ```
     ARG  CODE_VERSION=latest
     FROM base:${CODE_VERSION}
     CMD  /code/run-app

     FROM extras:${CODE_VERSION}
     CMD  /code/run-extras
     ```

     "ARG"声明在"FROM"之前是在构建阶段的外边，所以他不能再“FROM”之后的任何指令中使用。在第一个“FROM”之前用“ARG”的默认值，在构建阶段里边都没有用这个值。
     ```
     ARG VERSION=latest
     FROM busybox:$VERSION
     ARG VERSION
     RUN echo $VERSION > image_version
     ```

     2. VOLUME

     “VOLUME”指令通过指定的名称设置挂载点，并将其标记为从本机主机或其他容器保存外部挂载的卷。这个值可以是一个JSON数组，如 VOLUME ["/var/log/"]，或者是带多个参数的字符串，例如：VOLUME /var/log 或 VOLUME /var/log /var/db 。

     3. ADD

     ADD有两种形式：
     * ADD <src>... <dest>
     * ADD ["<src>",... "<dest>"]（这种形式是包含空格的路径所必须的）

     说明：复制本机的文件、目录、远程文件，添加到指定容器目录，支持GO的正则模糊匹配。路径是绝对路径，不会自动创建。如果源是一个目录，只会复制目录下的内容，目录本身不会复制。ADD命令会将复制的压缩文件夹自动解压，这也是与COPY命令最大的不同。

     4. RUN

     RUN有两种形式：

     * RUN <command> （shell形式，在shell中运行，在Linux中默认/bin/sh -c 或者 在Windows默认为 cmd /S /C）
     * RUN ["executable", "param1", "param2"] （exec形式）

     "RUN"执行完成之后会成为一个新的镜像，这里也是指镜像的分层结构，一句RUN就是一层，相当于一个版本。就是之前说的缓存原理。Docker镜像层是只读的，所以你如果第一句安装了软件，用完在后边一句删除是不可能的。所以这种情况要用一句RUN命令中完成，可以通过&符号连接多个RUN语句。RUN后边必须用双引号，不能用单引号（没引号貌似不要紧），command是不会调用shell的，所以也不会继承相应的变量，要查看输入RUN "sh" "-c" "echo" "$HOME"，而不是RUN "echo" "$HOME"。

     5. ENV

     用法： ENV <key> <value>
           ENV <key>=<value> ...

     设置容器的环境变量，可以让其后边的RUN命令使用，容器运行的时候这个变量也会保留。


     6. ENTRYPOINT

     “ENTRYPOINT”有两种形式
     * ENTRYPOINT ["executable", "param1", "param2"] （exec形式，首选）
     * ENTRYPOINT command param1 param1 （shell形式）

     这个命令和CMD命令一样，唯一的区别是不能被docker run命令的执行命令覆盖，如果要覆盖需要带上选项--entrypoint，如果有多个选项，只有最后一个会生效。


     7. -Djava.security.egd=file:/dev/./urandom

     这个系统属性egd表示熵收集守护进程(entropy gathering daemon)，但这里值为何要在dev和random之间加一个点呢？是因为一个jdk的bug，在这个bug的连接里有人反馈及时对 securerandom.source 设置为/dev/urandom它也仍然使用的/dev/random，有人提供了变通的解决方法，其中一个变通的做法是对securerandom.source设置为/dev/./urandom才行。也有人评论说这个不是bug，是有意为之。

### 6. 编写docker-compose.yml
```
# 版本
version : '2'
services:
  # springvideo自己定义的，就是服务名称
  springvideo:
    # build指定Dockerfile的文件夹路径
    build:
      context: .
      dockerfile: src/main/docker/Dockerfile
    # 重启
    restart: always
    # 端口映射，前者是宿主机的，后者是容器的
    ports:
      - "8080:8080"
    # 挂载路径：前者是宿主机的路径，后者是容器路径
    volumes:
      - ./dataSummary/springboot:/home/springboot/
```

### 7. 部署

* 先把程序打包
```
mvn package
```

* 编译
```
docker-compose build
```

* 部署
```
docker-compose up
```
