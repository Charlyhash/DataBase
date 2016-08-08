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

MySQL数据库中的表可以用不同的方式存储，根据需求的不同，选择不同的存储方式，是否进行事务处理。查询方法为：

SHOW ENGINES;
```
mysql> SHOW ENGINES \G
*************************** 1. row ***************************
      Engine: FEDERATED
     Support: NO
     Comment: Federated MySQL storage engine
Transactions: NULL
          XA: NULL
  Savepoints: NULL
*************************** 2. row ***************************
      Engine: MRG_MYISAM
     Support: YES
     Comment: Collection of identical MyISAM tables
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 3. row ***************************
      Engine: MyISAM
     Support: YES
     Comment: MyISAM storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 4. row ***************************
      Engine: BLACKHOLE
     Support: YES
     Comment: /dev/null storage engine (anything you write to it disa
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 5. row ***************************
      Engine: CSV
     Support: YES
     Comment: CSV storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 6. row ***************************
      Engine: MEMORY
     Support: YES
     Comment: Hash based, stored in memory, useful for temporary tabl
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 7. row ***************************
      Engine: ARCHIVE
     Support: YES
     Comment: Archive storage engine
Transactions: NO
          XA: NO
  Savepoints: NO
*************************** 8. row ***************************
      Engine: InnoDB
     Support: DEFAULT
     Comment: Supports transactions, row-level locking, and foreign k
Transactions: YES
          XA: YES
  Savepoints: YES
*************************** 9. row ***************************
      Engine: PERFORMANCE_SCHEMA
     Support: YES
     Comment: Performance Schema
Transactions: NO
          XA: NO
  Savepoints: NO
9 rows in set (0.05 sec)
````
可以看出有9个存储引擎。
- Engine: InnoDB 存储引擎的名字
- Support: DEFAULT 是否支持该类引擎
- Comment: Supports transactions, row-level locking, and foreign k 对该引擎的评论
- Transactions: YES 是否支持事务处理
- XA: YES 是否分布式交易处理的XA规范 
- Savepoints: YES 是否支持保持点。

InnoDB为默认存储引擎。

查询MySQL支持的存储引擎。
* Variable_name：存储引擎的名字
* Value：是否支持。YES表示支持，NO表示不支持，DISABLED表示支持但没有开启

```
mysql> SHOW VARIABLES LIKE "have%";
+----------------------+----------+
| Variable_name        | Value    |
+----------------------+----------+
| have_compress        | YES      |
| have_crypt           | NO       |
| have_dynamic_loading | YES      |
| have_geometry        | YES      |
| have_openssl         | DISABLED |
| have_profiling       | YES      |
| have_query_cache     | YES      |
| have_rtree_keys      | YES      |
| have_ssl             | DISABLED |
| have_symlink         | YES      |
+----------------------+----------+
10 rows in set (0.00 sec)
```

#### InnoDB存储引擎

InnoDB给MySQL的表提供了事务、回滚、崩溃修复能力和多版本并发控制的事务安全。
* 支持自动式增长列AUTO_INCRECENT
* 支持外键
* 创建的表存储在.frm文件中
* 提供了良好的事务管理、崩溃修复和并发控制。缺点是读写效率稍差。

#### MyISAM存储引擎

