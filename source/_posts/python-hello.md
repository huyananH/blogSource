---
title: 在MAC上安装Python3，编写Hello，World。
date: 2017-11-16 09:33:38
comments: true
tags:
  -MAC
  -Python3
---

<img src="/assets/postLog/pythonHelloLog.jpeg" width="350px" height="350px">

### 1. 前言

这两天真的是觉得整个人都特别不好。

<!-- more -->

### 2. 在MAC上下载Homebrew

*-- 在[Homebrew官网](https://brew.sh/)查看详细信息*

在Shell命令行输入以下命令
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 3. 安装Python3

在Shell命令行输入以下命令
```
brew install python3
```

### 4. 检查Python3是否安装成功

在Shell命令行输入以下命令
```
python -V
```

### 5. 编写HelloWorld

* 创建一个hello.py
```
print("Hello,World")
```

* 运行程序
```
python3 hello.py
```
* 运行结果

Hello,World
