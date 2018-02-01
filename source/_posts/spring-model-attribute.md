---
title: @ModelAttribute spring_model_attribute
date: 2018-01-30 13:47:11
tags:
  - Spring
---

### 一、 前言

今天写程序碰到了一个错误，如下图

![错误](/assets/postImg/modelattributeLogo.jpg

### 二、 @ModelAttribute 的应用场景

这个注解有两种用法，一种是直接标注在方法上，另一种呢，是标注在方法的参数中

### 三、 标注在方法上

当同一个控制器中有任意一个方法被@ModelAttribute注解标记，页面请求只要进入这个控制器，不管请求那个方法，均会先执行被@ModelAttribute标记的方法，所以我们可以用@ModelAttribute注解的方法做一些初始化操作。当同一个控制器中有多个方法被@ModelAttribute注解标记，所有被@ModelAttribute标记的方法均会被执行，按先后顺序执行，然后再进入请求的方法。

```
package cn.dfusion.mach.auth.controller;

import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@Api( description = "早期保健记录")
public class ChdEarlyChildCheckController {

    @Autowired
    private ChdEarlyChildCheckService checkService;

    @ModelAttribute
    public void init() {
        System.out.println("先执行这个");
    }

    @ModelAttribute
    public void init2() {
        System.out.println("先执行这个02");
    }

    @ApiOperation(value = "根据id删除保健记录")
    @DeleteMapping("/deleteCheckById")
    public @ResponseBody
    Message deleteCheckById(@RequestParam String checkId) {
        boolean flag = checkService.deleteCheckById(Long.parseLong(checkId));
        if (flag) {
            Message message = new Message(200,"保健记录删除成功");
            return message;
        }else {
            Message message = new Message(201, "保健记录删除失败");
            return message;
        }
    }
  }
```
运行结果如下：
![@ModelAttribute加在方法上](/assets/postImg/modelattributemethod.jpg)

### 四、 标注在方法的参数中
