---
title: Dockerfile用法
date: 2017-11-27 09:38:56
tags:
  - Dockerfile
---

<img src="/assets/postLog/dockerReferenceLog.jpeg" width="350px" height="350px">
### 1. 前言

Docker可以从Dockerfile读取指令自动生成镜像，Dockerfile是一个文本文档，其中包含了生成镜像所需要的所有命令。用 docker build 用户可以创建自动连续执行几个命令行的命令。

<!-- more -->

本页描述了你可以在Dockerfile使用的命令。当你读完这页之后，参考Docker *--[最佳实践](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/)* 写。

### 2. 用法

Docker build 命令从 Dockerfile 和上下文构建一个镜像。构建上下文是在指定的位置 PATH 或 URL。PATH 是本地文件系统的一个目录。URL是一个Git仓库的位置。

上下文被递归处理。所以，PATH 包含任何子目录，URL 包含仓库以及其子模块。这个例子展示了一个使用当前目录作为上下文的构建命令：
```
docker build .
```

构建有Docker保护进程运行，而不是用CLI运行。构建工程所做的第一件事是将整个上下文（递归地）发送到守护进程。在大多数情况下，最好以空目录作为上下文，并将Dockerfile保存在该目录中。仅添加构建Dockerfile所需的文件。

> ⚠️警告：不要用根目录，/ 作为 PATH ，因为它会导致生成到你硬盘驱动器的内容全部传输到守护进程。

要在构建上下文使用文件，Dockerfile 引用指令指定文件，例如 COPY 指令。为了增加构建的性能，通过将 .dockerignore 文件添加到上下文目录来排除文件和目录。有关 *-- [创建 .dockerignore 文件的信息](https://docs.docker.com/engine/reference/builder/#dockerignore-file)，请参考此页面上的文档*

传统上，Dockerfile被称为Dockerfile 并且位于上下文的根目录中。你用 -f 标记 docker build 在你的系统文件中指向的Dockerfile.
```
docker build -f /path/to/a/Dockerfile .
```

如果构建成功，你一颗指定仓库和保存新镜像的tag。
```
docker build -t shykes/mayapp .
```

构建之后要把镜像添加到多个仓库中，当你执行 build 命令时添加多个 -t 参数：
```
docker build -t shykes/myapp:1.0.2 -t shykes/myapp:latest .
```

在Docker守护进程运行Dockerfile命令之前，会进行初步的验证Dockerfile，如果语法有错误，返回一个错误：
```
$ docker build -t test/myapp .
Sending build context to Docker daemon 2.048 kb
Error response from daemon:Unknown instruction:RUNCMD
```

Docker 守护进程逐个运行 Dockerfile命令，在必要时将每条指令的结果交给新的镜像，最后输出新镜像ID，Docker守护进程自动清除你发送的上下文。

请注意：每条指令都是独立运行的，并且会创建一个新的镜像， RUN cd /tmp 不会作用到下一条指令。

只要有可能，Docker将重新使用中间镜像（缓存），使 docker build 有显著的加速。这是通过控制台输出使用缓存消息。（对于更多的信息，看 *--[构建缓存](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#build-cache)* 请参考最佳实践 Dockerfile ）
