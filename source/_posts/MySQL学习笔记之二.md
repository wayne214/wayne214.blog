---
title: MySQL学习笔记之二
date: 2019-01-17 12:08:28
tags: "数据库"
---
数据库的操作总结就是：增删改查(CURD),今天记录一下基础的检索查询工作。
> 检索MySQL
## 1.查询表中所有的记录
```
mysql> select * from apps;
+----+------------+-----------------------+---------+
| id | app_name   | url                   | country |
+----+------------+-----------------------+---------+
|  1 | QQ APP     | http://im.qq.com      | CN      |
|  2 | 微博 APP   | http://weibo.com      | CN      |
|  3 | 淘宝 APP   | http://www.taobao.com | CN      |
+----+------------+-----------------------+---------+
3 rows in set (0.00 sec)
```

<!-- more -->
## 2. 查询表中某列(单列)的记录
```
mysql> select app_name from apps;
+------------+
| app_name   |
+------------+
| QQ APP     |
| 微博 APP   |
| 淘宝 APP   |
+------------+
3 rows in set (0.00 sec)
```
## 3.检索表中多列的记录,列名之间用逗号分开
```
mysql> select id, app_name from apps;
+----+------------+
| id | app_name   |
+----+------------+
|  1 | QQ APP     |
|  2 | 微博 APP   |
|  3 | 淘宝 APP   |
+----+------------+
3 rows in set (0.00 sec)
```
## 4.检索不重复的记录，distinct关键字
```
mysql> select * from apps;
+----+------------+-----------------------+---------+
| id | app_name   | url                   | country |
+----+------------+-----------------------+---------+
|  1 | QQ APP     | http://im.qq.com      | CN      |
|  2 | 微博 APP   | http://weibo.com      | CN      |
|  3 | 淘宝 APP   | http://www.taobao.com | CN      |
|  4 | QQ APP     | http://im.qq.com      | CN      |
+----+------------+-----------------------+---------+
4 rows in set (0.00 sec)
上面表中是所有的数据，可以看到第1条和第4条数据是一样的，如果想要检索时不想出现重复的数据，可以使用distinct关键字，并且需要放在所有列的前面
mysql> select distinct app_name from apps;
+------------+
| app_name   |
+------------+
| QQ APP     |
| 微博 APP   |
| 淘宝 APP   |
+------------+
3 rows in set (0.00 sec)
```
## 5.限制检索记录的数量， limit关键字
```
mysql> select * from apps limit 2;
+----+------------+------------------+---------+
| id | app_name   | url              | country |
+----+------------+------------------+---------+
|  1 | QQ APP     | http://im.qq.com | CN      |
|  2 | 微博 APP   | http://weibo.com | CN      |
+----+------------+------------------+---------+
2 rows in set (0.00 sec)

limit后面可以跟两个值, 第一个为起始位置, 第二个是要检索的行数
mysql> select * from apps limit 1, 2;
+----+------------+-----------------------+---------+
| id | app_name   | url                   | country |
+----+------------+-----------------------+---------+
|  2 | 微博 APP   | http://weibo.com      | CN      |
|  3 | 淘宝 APP   | http://www.taobao.com | CN      |
+----+------------+-----------------------+---------+
2 rows in set (0.00 sec)

```

## 5.limit关键字和offset配合使用
```
limit可以配合offset使用, 如limit 2 offset 1(从行1开始后的2行,默认行数的角标为0)
mysql> select * from apps limit 2 offset 1;
+----+------------+-----------------------+---------+
| id | app_name   | url                   | country |
+----+------------+-----------------------+---------+
|  2 | 微博 APP   | http://weibo.com      | CN      |
|  3 | 淘宝 APP   | http://www.taobao.com | CN      |
+----+------------+-----------------------+---------+
2 rows in set (0.00 sec)
```