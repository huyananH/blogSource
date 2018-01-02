---
title: Mysql 开发技巧
date: 2017-12-19 09:10:14
tags:
  - Mysql
---

### 1. 目录
* 如何进行行列的转换
* 如何生成唯一序列号
* 如何删除重复数据

<!-- more -->

### 2. 如何进行行列的转换
#### 1. 行转为列

* 场景
  * 报表统计
  ![行转列的场景](/assets/postImg/rowchangecol.jpg)
  Sql语句实现
  ```
  select a.user_name,kills from user1 a join user_kills b on a.id = b.user_id;
  ```
  * 汇总显示
  ![行转列的场景](/assets/postImg/rowchangecol1.jpg)
  Sql语句实现
  ```
  select a.user_name,sum(kills) from user1 a join user_kills b on a.id = b.user_id group by a.user_name;
  ```

* 处理方式
![打怪数](/assets/postImg/killNumList.jpg)
  * 使用Cross join实现（自连接）
  Sql实现(复杂的，效率低，)
  ```
  select * from
  (
  select sum(kills) as '猪八戒' from user1 a join user_kills b on a.id = b.user_id and a.user_name = '猪八戒'
  ) a cross join(
  select sum(kills) as '孙悟空' from user1 a join user_kills b on a.id = b.user_id and a.user_name = '孙悟空'
  ) b cross join(
  select sum(kills) as '猪八戒' from user1 a join user_kills b on a.id = b.user_id and a.user_name = '孙悟空'
  ) c
  ```

  * 使用Case语句实现
  Sql语句实现
  ```
  select sum(case when a.user_name='孙悟空' then kills end) as '孙悟空',
  sum(case when a.user_name='猪八戒' then kills end) as '猪八戒',
  sum(case when a.user_name='沙僧' then kills end) as '沙僧'
  from user1 a join user_kills b on a.id = b.user_id;
  ```

#### 2. 列转行
* 场景
  * 属性拆分
  ![属性拆分](/assets/postImg/propertySplit.jpg)
  * ETL数据处理
  ![ETL数据处理](/assets/postImg/ETLDispose.jpg)

* 使用序列表处理
  * 需要生成一张序列表
  ```
  create table tb_sequence(id int auto_increment not null, primary key(id));
  ```
  * 添加数据
  ```
  insert into tb_sequence values(),(),(),(),(),();
  ```
  * 查询序列表
  ```
  select * from tb_sequence;
  ```
