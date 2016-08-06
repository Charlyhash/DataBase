本章内容有
* 创建数据库
* 删除数据库
* 数据库的存储引擎
* 如何选择存储引擎

##创建数据库

创建：CREATE DATABASE + 数据库名字；
显示：SHOW DATABASES;

```
CREATE DATABASE first_database;

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| first_database     |
| mysql              |
| performance_schema |
| test               |
+--------------------+
5 rows in set (0.08 sec)
```

#### 删除数据库

删除方法：DROP DATABASE + 数据库名；

```
mysql> CREATE DATABASE mybooks;
Query OK, 1 row affected (0.00 sec)

mysql>  SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| first_database     |
| mybooks            |
| mysql              |
| performance_schema |
| test               |
+--------------------+
6 rows in set (0.00 sec)

mysql> DROP DATABASE mybooks;
Query OK, 0 rows affected (0.08 sec)

mysql>  SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| first_database     |
| mysql              |
| performance_schema |
| test               |
+--------------------+
5 rows in set (0.00 sec)
```

## 数据库存储引擎

#### MySQL存储引擎简介

MySQL数据库中的表可以用不同的方式存储，根据需求的不同，选择不同的存储方式，是否进行事务处理。

```
mysql> SHOW ENGINES;
+--------------------+---------+------------------------------------------------
----------------+--------------+------+------------+
| Engine             | Support | Comment
                | Transactions | XA   | Savepoints |
+--------------------+---------+------------------------------------------------
----------------+--------------+------+------------+
| FEDERATED          | NO      | Federated MySQL storage engine
                | NULL         | NULL | NULL       |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables
                | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine
                | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to
 it disappears) | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine
                | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for tempor
ary tables      | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine
                | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and f
oreign keys     | YES          | YES  | YES        |
| PERFORMANCE_SCHEMA | YES     | Performance Schema
                | NO           | NO   | NO         |
+--------------------+---------+------------------------------------------------
----------------+--------------+------+------------+
````