---
title: github 博客绑定自己的域名
date: 2018-01-13 00:19:33
tags:
  - 博客
---

<img src="/assets/postImg/blogdomainnameLogo.jpeg" width="350px" height="350px">

### 一、 前言

这段时间真的好忧伤......

我不愧是同学说的那样，演技派的，赶紧收拾收拾出道吧......戏精。

请自己脑补一个大笑的表情谢谢。要不还要麻烦我截张图过来。

<!-- more -->

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=444323371&auto=1&height=66" style="margin-left: -10px;"></iframe>

### 二、 买域名

1. *- [请自行到万网购买](https://wanwang.aliyun.com/?utm_content=se_1101880)*
![万网](/assets/postImg/wanwangpage.jpg)

### 三、 配置DNS

1. 购买完，上图点击右上角的控制台，进入以下页面
![万网控制台](/assets/postImg/wanwangcontro.jpg)

2. 点击右侧已经买好的域名，将域名的DNS设置为DNSPod的免费DNS地址：f1g1ns1.dnspod.net 和 f1g1ns2.dnspod.net
![修改页面](/assets/postImg/wanwangupdatedns.jpg)

### 四、 配置域名解析器

1. *-到[DNSPod](https://www.dnspod.cn) 后台注册一个账号*

2. 进入控制台，点击域名解析，添加域名
![添加域名](/assets/postImg/dnspodinsert.jpg)

3. 按照如下，添加记录，倒数第二个勾掉的不用。最后一个的记录值，是博客github的地址。
![添加记录](/assets/postImg/dnspod.jpg)

### 五、 配置Hexo

在本地hexo文件目录的source目录下新建一个文本文件，命名为CNAME，没有后缀，并且填入域名地址：
![配置域名](/assets/postImg/domainhexo.jpg)

然后，上传部署到github上，就此，绑定域名就已完成。

注意：
在DNSPod配置域名解析会需要些时间，一般是48h以内，耐心等待下
