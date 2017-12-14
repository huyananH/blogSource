---
title: Mysql开发技巧
date: 2017-12-14 09:20:51
tags:
  - Mysql
---

### 1. 前言

Sql语句我都忘的差不多了，温习一下
这一部分是Join从句

<!-- more -->

### 2. Join从句
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
