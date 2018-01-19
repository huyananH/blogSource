---
title: Hexo 博客添加文章阅读量
date: 2018-01-08 12:25:59
tags:
  - 博客
---

<img src="/assets/postImg/hexoreaddingcountLogo.jpeg" width="350px" height="350px">

### 一、 前言

前段时间，我和我妈说，下次回去给抱只猫回去，我妈崩溃了 #照顾猫太累了#

我和我姐说：“我准备养只猫，名字都想好了，就叫嘟嘟”，然后，我姐崩溃了 #我小外甥叫嘟嘟#

哈哈哈哈......

接下来，回到正题......

<!-- more -->

### 二、*- [配置LeanCloud](https://leancloud.cn/)*

1. 点击右上角进入控制台
![leanCloud官网](/assets/postImg/leanCloud.jpg)

2. 点击左上角的应用，点击创建新应用
![创建新应用](/assets/postImg/leanController.jpg)

3. 创建新应用
![填写名字](/assets/postImg/AppName.jpg)

4. 创建完成，点击进入
![点击进入](/assets/postImg/createSuc.jpg)

5. 点击创建Class
为了和NexT主题的修改兼容，所以Class名字必须是Counter
![创建Class](/assets/postImg/createClass.jpg)

6. 创建好之后，点击左侧菜单的设置，点击应用Key，获得app_id,app_key
![应用Key](/assets/postImg/settingLean.jpg)

7. 在主题配置文件_config.yml文件中配置

```
leancloud_visitors:
  enable: true
  app_id: JtaBBp4RC5QKPz9wTQ8pm1h2-gzGzoHsz
  app_key: KTJXOMQTftuPSbNfh3kSjnUm
```

8. 重新部署，就可以看到结果
