---
#layout: post
#标题配置
title:  SQL入门
#时间配置
date:   2019-10-08 17:44:00 +0800
#大类配置
categories: document
#小类配置
tag: [SQL, Tutorial]
#网易云音乐，只能播放无版权保护的
music-id: 28258623
---

> SQL的简介

<!-- more -->

# 介绍

&emsp;&emsp;最近选修了一门 “数据库技术应用”，遂入门了数据库。下面大部分的内容摘自《MySQL 必知必会》，这本书很好，推荐大家去看看，我只讲部分重点内容：

- [ ] 创建数据库
- [ ] 检索数据
- [ ] 排序检索数据
- [x] 过滤数据
- [ ] 正则表达式
- [ ] 插入数据

&emsp;&emsp;我是在 Linux 上安装 MySQL 来学习的，所以下面都是命令行的形式。



# 创建数据库

```mysql
CREATE DATEBASE <数据库名>;
```

创建一个数据库。

```mysql
CREATE DATABASE IF NOT EXISTS <数据库名> DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

创建数据库，该命令的作用：

1. 如果数据库不存在则创建，存在则不创建。
2. 创建RUNOOB数据库，并设定编码集为utf8



# 删除数据库

```mysql
DROP DATABASE <数据库名>;
```

删除数据库。



# 选择数据库

```mysql
USE <数据库名>;
```

选择数据库（即打开数据库）



# 创建数据表

```mysql
CREATE TABLE <表名> (column_name column_type, ...);
```

创建数据表，括号里面可以指定多个**列名**（必需）、**数据类型**（必需）和**约束条件**，还可以指定**主值**、**处理引擎**、**字符集**等。比如：

```mysql
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```



# 删除数据表

```mysql
DROP TABLE <表名>
```



# 检索数据

## SELECT 语句

### 检索列

```mysql
SELECT 列1，列2 FROM <表名>
```

### 检索所有列

```mysql
SELECT * FROM <表名>
```

`*` 是通配符，可以匹配多个名字

如果后面有多个表，则结果是表相乘

```mysql
SELECT * FROM <列1，列2>
```

### 检索不同行

如果要删除列中的重复行，可以用：

```mysql
SELECT DISTINCT 列1，列2 FROM <表名>
```

这要就会删去 “列1，列2” 这两个属性相同的重复行

`DISTINCE` 只能放在列名的前面，并且要应用于列出的所有列。

### 限定表名

如果列在多个表内，则在同时检索这两个表时，需要在列名前面加上限制表名：

```mysql
SELECT 表.列 FROM 表1，表2
```



## LIKE 语句

如果要搜索特定特征的文本，可以使用 LIKE 语句：

```mysql
SELECT 列 FROM 表 WHERE 列 LIKE "匹配文本"
```

> 注：特殊的是，如果匹配文本中没有统配符，则即使列中的值完全等于匹配文本，也不会有输出

### 通配符

MySQL 中有三种通配符：

1. `%` 匹配 0~n 个任意字符
2. `_` 匹配 1 个任意字符



## 正则表达式

正则表达式和 `LIKE` 语句相类似：

```mysql
SELECT 列 FROM 表 WHERE 列 REGEXP "匹配模式"
```



# 排序数据

## ORDER BY 语句

在 `SELECT ... FROM ... WHERE ...` 后面加上 `ORDER BY`，可以将数据按一定顺序输出：

```mysql
SELECT 列 FROM 表 WHERE 判断语句 ORDER BY 列
```

### 多个列排序

### 排序方向

### 





# 过滤数据

## WHERE 语句

使用 `SELECT` 会检索所有行，如果要提取特定的行，可以用 `WHERE`：

```mysql
SELECT 列1，列2 FROM 表 WHERE 判断语句
```



## 操作符

| 操作符                    | 说明                               |
| ------------------------- | ---------------------------------- |
| =                         | 等于                               |
| <> 或 !=                  | 不等于                             |
| <，<= ，>，>=             | 小于，小于或等于，大于，大于或等于 |
| BETWEEN value1 AND value2 | 在指定值之间（包括两端）           |
| IS NULL                   | 是空值                             |



## 逻辑符

| 操作符 | 说明 |
| ------ | ---- |
| AND    | 与   |
| OR     | 或   |
| NOT    | 非   |

> 注：在 MySQL 中，NOT 只能对 IN, BETWEEN, EXISTS 取反，但在 SQL Server 中，NOT 可以对各种条件取反。

示例：

```mysql
SELECT 列 FROM 表 WHERE 条件1 AND 条件2
```

```sql
SELECT 列 FROM 表 WHERE 条件1 OR 条件2
```

```mysql
SELECT 列 FROM 表 WHERE NOT IN/BETWEEN/EXISTS
```

优先级：（和大多数 SQL 语言一样）在 MySQL 中，AND 的优先级大于 OR 的优先级。可以通过括号改变优先级。



# 汇总数据

用于计算数据的和、平均值等。

## 聚集函数

聚集函数作用在一个 行组 上，计算并返回一个值，包括：

| 函数     | 说明   |
| -------- | ------ |
| AVG( )   | 平均值 |
| COUNT( ) | 行数   |
| MAX( )   | 最大值 |
| MIN( )   | 最小值 |
| SUM( )   | 和     |

> 注：
>
> 1. `AVG( )` 只能用于单列，要勇于多列，要用多个 `AVG( )`
> 2. `COUNT(*)` 会包括空行，但 `COUNT(column)` 则不包括空行
> 3. `MAX( )` 会返回数字或日期的最大值，对于文本，返回最后一行。

示例：

```mysql
SELECT AVG(列) AS 别名 FROM 表 #给结果提供一个别名，方便使用
```

```mysql
SELECT COUNT(*) AS 别名 FROM 表
```

