---
title: 基本知识 Java
date: 2017-12-06 09:45:35
tags:
  - Java
---

### 前言

我希望自己的代码写的更规范一点，所以，我开始看阿里巴巴的Java开发手册

<!-- more -->

### 1. 编程规约

#### 1. 命名规则

* 美元符号、下划线 不能开头，不能结尾

* 类名 UpperCamelCase风格，除了DO、BO、VO、AO、PO、DTO

正例：UserPO
反例：UserPo

* 方法名、参数、成员变量、局部变量 lowerCamelCase风格

正例：inputUserName
反例：InputUserName

* 常量 全部大写，单词之间用下划线连接，不要嫌名字过长

正例：MIN_STOCK_COUNT
反例：MIN_COUNT

* 抽象类名 以Abstract或Base开头
  异常类名 以Exception结尾
  测试类名 以要测试的类的类名开始，以Test结尾

* 各层命名规则

  * Service/Dao层方法命名规则
    * 获得单个对象调用get前缀
    * 获得多个对象调用list前缀
    * 获得统计值的方法用count做前缀
    * 插入的方法用save/insert做前缀
    * 删除的方法用remove/delete做前缀
    * 修改的方法用update做前缀

  * 领域模型命名规则
    * 数据对象：XxxDO，Xxx为表名
    * 数据传输类：XxxDTO，Xxx为业务领域相关的名称
    * 展示对象：XxxVO，Xxx一般为网页的名称
    * POJO是DO、BO、VO、DTO的统称，一般不用POJO作为后缀

#### 2. 常量定义

* 不允许任何魔法值（即未预先定义的常量）出现在代码中

反例:String key = "Id#taobao_" + tradeId;
cache.put(key, value);

* long或者Long初始化时，使用大写的L，不要用小写的l，可能和数字1混淆
说明:Long a = 2l; 写的是数字的21，还是Long型的2?

* 不要使用一个常量类维护所有常量，按常量功能进行归类，分开维护。

说明：大而全的常量类，非得使用查找功能才能定位到修改的常量，不利于理解和维护。
正例：缓存相关常量放在类 CacheConsts 下;系统配置相关常量放在类 ConfigConsts 下。

* 如果变量值仅在一个固定范围内变化用 enum 类型来定义。 说明:如果存在名称之外的延伸属性使用 enum 类型，下面正例中的数字就是延伸信息，表示 一年中的第几个季节。
正例：

public enum SeasonEnum {
   SPRING(1), SUMMER(2), AUTUMN(3), WINTER(4);
   int seq;
   SeasonEnum(int seq){
       this.seq = seq;
   }
}

#### 3. 代码格式

* if/for/while/switch/do 等保留字与括号之间都必须加空格。

* 采用 4 个空格缩进，禁止使用 tab 字符。

说明：如果使用 tab 缩进，必须设置 1 个 tab 为 4 个空格。
    IDEA 设置 tab 为 4 个空格时， 请勿勾选Use tab character
    在Eclipse，必须勾选 insert spaces for tabs

* 注释的双斜线与注释内容之间有且只有一个空格

正例：
  // 输出一行字
  System.out.println("你猜");

* 单行字符数限制不超过 120 个，超出需要换行，换行时遵循如下原则:
  1) 第二行相对第一行缩进 4 个空格，从第三行开始，不再继续缩进，参考示例。
  2) 运算符与下文一起换行。
  3) 方法调用的点符号与下文一起换行。
  4) 方法调用时，多个参数，需要换行时，在逗号后进行。 5) 在括号前不要换行，见反例。

* 方法参数在定义或传入时，逗号后边必须有一个空格。

* 不同逻辑、不同语义、不同业务的代码之间插入一个空行分隔开来以提升可读性。 说明:没有必要插入多个空行进行隔开。

* IDE的text file encoding设置为UTF-8; IDE中文件的换行符使用Unix格式，不要使用 Windows 格式。
IDEA下的修改图解
![](/assets/postImg/ideaLineSepara.jpg)

#### 4. OOP规约

* 避免通过一个类的对象调用它的静态变量或静态方法，无谓增加编译器的解析成本，直接用类名访问。

* 所有覆写的方法，都必须加@Override
