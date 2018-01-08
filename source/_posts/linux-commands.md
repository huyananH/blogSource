---
title: Linux 命令总结
date: 2017-12-01 10:06:27
tags:
  - Linux
---

<p><img src="/assets/postImg/linuxcommandsLogo.jpeg" width="350px" height="350px"></p>

### 1. 前言

现在的监考已经这么严了，幸好我已经毕业了......

<!-- more -->

Linux下的命令特别多，这篇我总结一下经常用的命令。
顺便自己也记忆一下，每次用的时候都想不起来。

## 一、文件管理

### 1. chmod 命令

Linux/Unix 的文件调用权限分为3级：文件拥有者、群组、其他。
1. 文件如何被他人所调用，命令格式如下：
```
chmod [-cfvR] [--help] [-version] model files...
```
参数详解
model设定字符，格式如下
```
[ugoa...][[+-=][rwxX]...][,...]
```
其中：
  * u：文件所有者，g：和文件所有者属于同一群组，o：其他人以外的人，a：三种都是。
  * +：增加权限，-：取消权限，=：唯一设定权限
  * r：可读，w：可写，x：可执行，X：当该文件是子目录或者已经设置及可执行。
其他：
       -c：若该文件权限确实已经更改，才显示其更改动作。
       -f：若该文件无法修改，也不显示错误讯息。
       -v：显示权限变更的详细资料。
       -R：对当前目录下的所有文件与子目录进行相同权限变更（以递归地方式进行变更）
   --help：显示辅助说明
 -version：显示版本

 例子：
 * file1.text设置所有人皆可读
```
 chmod ugo+r file1.txt
```
Or
```
chmod a+r file1.txt
```
* 将文件 file1.txt 与 file2.txt 设为该文件拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入 :
```
chmod ug+w,o-w file1.txt file2.txt
```
* 将 ex1.py 设定为只有该文件拥有者可以执行 :
```
chmod u+x ex1.py
```
* 将目前目录下的所有文件与子目录皆设为任何人可读取 :
```
chmod -R a+r *
```
2. 此外chmod也可以用数字来表示权限如 :
```
chmod 777 file
```
语法为：
```
chmod abc file
```
其中：
  * a：文件拥有者（User），b：与文件拥有者属于同一个Group的，c：其他以外的人（Other）
  * r=4，w=2，x=1
    * 若要rwx属性，则r+w+x=7,
    * 若要rw属性，则r+w=6
    * 若要rx属性，则r+x=5

例子：
```
chmod 777 file
```
And
```
chmod a+rwx file
```
效果相同

```
chmod ug+rwx,o+x file
```
And
```
chmod 771 file
```
效果相同

### 2. cat 命令
语法规则：
```
cat [-AbeEnstTuv] [--help] [--version] fileName
```
参数：
  -n 或 --number：从1开始，对所有的输出行进行编号。
  -b 或 --number-nonblank：与-n特别像，只是-b对与空白行不编号。
  -s 或 --squeeze-blank：遇到两行以上空行，转为一行空行。
  -v 或 --show-nonprinting：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。
  -E 或 --show-ends：每行结束处，显示$
  -T 或 --show-tabs：Tab显示^I
  -e：等价于-vE
  -t：等价于-vT
  -A 或 --show-all：等价于-vET



例子：
* 把 file1.txt 的文档内容加上行号后输入 file2.txt 这个文档里：
```
cat -n file1.txt>file2.txt
```

* 把 textfile1 和 textfile2 的文档内容加上行号（空白行不加）之后将内容附加到 textfile3 文档里：
```
cat -b textfile1 textfile2>>textfile3
```

* 清空 /etc/test.txt 文档内容：
```
cat /dev/null>test.txt
```

## 二、磁盘管理
### 1. cd 命令
切换当前目录到dirname
```
cd [dirname]
```

例子：
* 跳到 /usr/bin/ :
```
cd /usr/bin/
```

* 跳到自己的 home 目录 :
```
cd ~
```
Or
```
cd
```

* 跳到目前目录的上上两层 :
```
cd ../..
```
