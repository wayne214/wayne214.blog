---
title: MySQL学习笔记之一
date: 2019-01-16 11:14:14
tags: "数据库"
---
[MySQL](https://dev.mysql.com/downloads/) 是最流行的关系型数据库管理系统，在 WEB 应用方面 MySQL 是最好的 RDBMS(Relational Database Management System：关系数据库管理系统)应用软件之一。
MySQL 是一个关系型数据库管理系统，由瑞典 MySQL AB 公司开发，目前属于 Oracle 公司。MySQL 是一种关联数据库管理系统，关联数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。
- MySQL 是开源的，所以你不需要支付额外的费用。
- MySQL 支持大型的数据库。可以处理拥有上千万条记录的大型数据库。
- MySQL 使用标准的 SQL 数据语言形式。
- MySQL 可以运行于多个系统上，并且支持多种语言。这些编程语言包括 C、C++、Python、Java、Perl、PHP、Eiffel、Ruby 和 Tcl 等。
- MySQL 对PHP有很好的支持，PHP 是目前最流行的 Web 开发语言。
- MySQL 支持大型数据库，支持 5000 万条记录的数据仓库，32 位系统表文件最大可支持 4GB，64 位系统支持最大的表文件为8TB。
- MySQL 是可以定制的，采用了 GPL 协议，你可以修改源码来开发自己的 MySQL 系统。

学习的开发环境： Mac, MySQL, Navicat(可视化SQL工具)
[Mac 安装MySQL数据库方式](https://www.cnblogs.com/xuyatao/p/6932885.html)
[Mac使用brew(Homebrew)安装mysql，附 Mac Nacicat Premium 破解版](https://blog.csdn.net/eeeecw/article/details/82769844?utm_source=blogxgwz4)

注意：Mysql对大小写不敏感, SQL语句末尾加分号“；”

<!-- more -->

> 使用MySQL
## 1.命令行连接MySQL服务器
```
mysql -u root -p
Enter password:******
```
## 2.创建数据库database
```
mysql> create DATABASE [数据库名称];
```
## 3.删除数据库
```
mysql> drop DATABASE [数据库名称];
```
## 4.选择或切换数据库
 ```
mysql> use RUNOOB;
Database changed
```
## 5.查看当前数据库
```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| runoob             |
| sys                |
+--------------------+
```

## 6.查看一个数据库中的所有的表
```
mysql> use runoob;
Database changed
mysql> show tables;
+------------------+
| Tables_in_runoob |
+------------------+
| apps             |
| employee         |
| runoob_tbl       |
| tcount_tbl       |
| websites         |
+------------------+
5 rows in set (0.00 sec)
```
## 7. 查看表中所有的列：
```
mysql>show columns from apps;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int(10)     | NO   | PRI | NULL    |       |
| app_name | varchar(20) | NO   |     | NULL    |       |
| url      | varchar(40) | NO   |     | NULL    |       |
| country  | varchar(20) | NO   |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```
## 8. 查看创建表的sql语句：
```
mysql> show create table apps;
+-------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                                                                |
+-------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| apps  | CREATE TABLE `apps` (
  `id` int(10) NOT NULL,
  `app_name` varchar(20) NOT NULL,
  `url` varchar(40) NOT NULL,
  `country` varchar(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
+-------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```
## 9. 查询创建数据库的sql语句
```
mysql> show create database runoob;
+----------+-----------------------------------------------------------------+
| Database | Create Database                                                 |
+----------+-----------------------------------------------------------------+
| runoob   | CREATE DATABASE `runoob` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+-----------------------------------------------------------------+
1 row in set (0.00 sec)
```
