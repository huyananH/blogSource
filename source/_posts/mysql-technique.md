---
title: Mysql 开发技巧
date: 2017-12-14 09:20:51
tags:
  - Mysql
---

<img src="/assets/postImg/mysqlTechniqueLogo.jpeg" width="350px" height="350px">

### 一、 前言

今天看了一个笑话，和一个天津人谈恋爱就像在说相声
>
我：新的一年，希望你更喜欢我
男朋友：那不废话么！
我：希望以后还能一起做很多事
男朋友：好嘞您

哈哈哈......笑死了

<!-- more -->

### 二 Join从句
* 内连接（INNER）
* 全外连接（FULL OUTER）
* 左外连接（LEFT OUTER）
* 右外连接（RIGHT OUTER）
* 交叉连接（CROSS）

#### 1. 内连接（INNER JOIN）

介绍：基于连接谓词将两张表（如：A和B）的列组合在一起，得出新的结果集。
简单的来说：就是找到两张表的公共部分。
语法：
```
select <select_list> from TableA A inner join TableB B on A.Key=B.Key;
```

#### 2. 左外连接（LEFT OUTER） ---左连接

返回包括左表中的所有记录和右表中联结字段相等的记录
语法：
```
select <select_list> from TableA A left join TableB B on A.Key=B.Key;
```

#### 3. 右外连接（RIGHT OUTER）---右连接

返回包括左表中的所有记录和右表中联结字段相等的记录
语法：
```
select <select_list> from TableA A right join Table B on A.Key=B.Key;
```

#### 4. 全外连接（FULL OUTER）

注意：Mysql不支持全外连接

可以查到两表中的全部数据，或者说是两表的公共部分。

语法：
错误❎
```
select <select_list> from TableA A full join Table B on A.Key=B.Key;
```

正确：✅
把左连接和右连接用UNION ALL连接
```
select <select_list> from TableA A left join TableB B on A.Key=B.Key
UNION ALL
select <select_list> from TableA A right join Table B on A.Key=B.Key;
```

#### 5. 交叉连接（CROSS JOIN）

返回A表和B表的乘积

注意：没有ON

语法：
```
select <select_list> from TableA A cross join TableB。
```

### 三、 如何进行行列的转换

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
