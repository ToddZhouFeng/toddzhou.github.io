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
- [ ] 过滤数据
- [ ] 正则表达式
- [ ] 插入数据

&emsp;&emsp;我是在 Linux 上安装 MySQL来学习的，所以下面都是命令行的形式。



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

创建数据表，括号里面可以指定多个**列名**、**数据类型**和**约束条件**，还可以指定**主值**。比如：

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

## SELECT语句

