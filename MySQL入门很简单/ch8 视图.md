
## 什么是视图

视图view是基于 SQL 语句的结果集的可视化的表，视图是由查询结果形成的一张虚拟表，视图也包含行和列，就像一个真实的表。使用视图查询可以使查询数据相对安全，通过视图可以隐藏一些敏感字段和数据，从而只对用户暴露安全数据。视图查询也更简单高效，如果某个查询结果出现的非常频繁或经常拿这个查询结果来做子查询，将查询定义成视图可以使查询更加便捷。

## MySQL创建视图

```
CREATE  [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}] 
	VIEW view_name [(column_list)] 
	AS select_statement 
	[WITH [CASCADED | LOCAL] CHECK OPTION];
```

* ALGORITHM表示视图选择算法
* select_statement表示选择的显示属性
* SELECT语句为查询语句，选出符合条件的记录
* WITH CHECK OPTION为可选参数，表示更新视图时要保证在该视图的权限范围之内。

> 属性：
> Select_priv:SELECT属性
> Create_view_priv:CREATE VIEW属性


##查看视图

查看视图基本信息：
```
DESCRIBE view_name;
SHOW TABLE STATUS LIKE view_name;
```
查看视图详细信息
```
SHOW CREATE VIE view_name;
```
查看表中的视图
```
SELECT * FROM information_schema.views;
```

## 修改视图

```
CREATE OR REPLACE [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}] 
	VIEW view_name [(column_list)] 
	AS select_statement 
	[WITH [CASCADED | LOCAL] CHECK OPTION];
```

```
ALTER [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}] 
	VIEW view_name [(column_list)] 
	AS select_statement 
	[WITH [CASCADED | LOCAL] CHECK OPTION];
```

## 删除视图

```
DROP VIEW view_name;
```

## 更新视图




例子：

1. 使用test数据库
```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| job                |
| mysql              |
| performance_schema |
| test               |
+--------------------+
5 rows in set (0.00 sec)

mysql> USE test;
Database changed
```
2. 创建work_info表
```
mysql> CREATE TABLE work_info(
    -> id INT(10) NOT NULL UNIQUE PRIMARY KEY,
    -> name VARCHAR(20) NOT NULL,
    -> sex VARCHAR(4) NOT NULL,
    -> age INT(5),
    -> address VARCHAR(50),
    -> tel VARCHAR(20));
Query OK, 0 rows affected (0.59 sec)
```
3. 插入数据
```
mysql> INSERT INTO work_info VALUES(1, 'ZHANG SAN','M', 18 , 'BEIJING','1234567'
);
Query OK, 1 row affected (0.07 sec)

mysql> INSERT INTO work_info VALUES(2, 'LI SI','M', 22 , 'BEIJING','2345678');
Query OK, 1 row affected (0.05 sec)

mysql> INSERT INTO work_info VALUES(3, 'WANG WU','F', 17 , 'HUNAN','3456789');
Query OK, 1 row affected (0.07 sec)

mysql> INSERT INTO work_info VALUES(4, 'ZHAO LIU','M', 25 , 'LIAONING','4567890'
);
Query OK, 1 row affected (0.10 sec)

mysql>  SELECT * FROM work_info
    -> ;
+----+-----------+-----+------+----------+---------+
| id | name      | sex | age  | address  | tel     |
+----+-----------+-----+------+----------+---------+
|  1 | ZHANG SAN | M   |   18 | BEIJING  | 1234567 |
|  2 | LI SI     | M   |   22 | BEIJING  | 2345678 |
|  3 | WANG WU   | F   |   17 | HUNAN    | 3456789 |
|  4 | ZHAO LIU  | M   |   25 | LIAONING | 4567890 |
+----+-----------+-----+------+----------+---------+
4 rows in set (0.00 sec)
```

4. 创建视图
```
mysql> CREATE ALGORITHM=MERGE VIEW
    -> info_view (id,name,sex,address)
    -> AS SELECT id, name, sex, address
    -> FROM work_info WHERE age > 20
    -> WITH LOCAL CHECK OPTION;
Query OK, 0 rows affected (0.12 sec)

mysql> SHOW CREATE VIEW info_view \G
*************************** 1. row ***************************
                View: info_view
         Create View: CREATE ALGORITHM=MERGE DEFINER=`root`@`localhost` SQL SECU
RITY DEFINER VIEW `info_view` AS select `work_info`.`id` AS `id`,`work_info`.`na
me` AS `name`,`work_info`.`sex` AS `sex`,`work_info`.`address` AS `address` from
 `work_info` where (`work_info`.`age` > 20) WITH LOCAL CHECK OPTION
character_set_client: gbk
collation_connection: gbk_chinese_ci
1 row in set (0.00 sec)


mysql> SELECT * FROM info_view;
+----+----------+-----+----------+
| id | name     | sex | address  |
+----+----------+-----+----------+
|  2 | LI SI    | M   | BEIJING  |
|  4 | ZHAO LIU | M   | LIAONING |
+----+----------+-----+----------+
2 rows in set (0.00 sec)

mysql> ALTER ALGORITHM=MERGE VIEW
    -> info_view (id, name, sex, address)
    -> AS SELECT id, name, sex, address
    -> FROM work_info WHERE age<20
    -> WITH LOCAL CHECK OPTION;
Query OK, 0 rows affected (0.07 sec)

mysql> SHOW CREATE VIEW info_view \G
*************************** 1. row ***************************
                View: info_view
         Create View: CREATE ALGORITHM=MERGE DEFINER=`root`@`localhost` SQL SECU
RITY DEFINER VIEW `info_view` AS select `work_info`.`id` AS `id`,`work_info`.`na
me` AS `name`,`work_info`.`sex` AS `sex`,`work_info`.`address` AS `address` from
 `work_info` where (`work_info`.`age` < 20) WITH LOCAL CHECK OPTION
character_set_client: gbk
collation_connection: gbk_chinese_ci
1 row in set (0.00 sec)

mysql> SELECT * FROM info_view;
+----+-----------+-----+---------+
| id | name      | sex | address |
+----+-----------+-----+---------+
|  1 | ZHANG SAN | M   | BEIJING |
|  3 | WANG WU   | F   | HUNAN   |
+----+-----------+-----+---------+
2 rows in set (0.00 sec)
```
5. 修改视图
```
mysql> UPDATE info_view SET sex='M' WHERE id=3;
Query OK, 1 row affected (0.08 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SELECT * FROM info_view;
+----+-----------+-----+---------+
| id | name      | sex | address |
+----+-----------+-----+---------+
|  1 | ZHANG SAN | M   | BEIJING |
|  3 | WANG WU   | M   | HUNAN   |
+----+-----------+-----+---------+
2 rows in set (0.00 sec)

mysql> SELECT * FROM work_info;
+----+-----------+-----+------+----------+---------+
| id | name      | sex | age  | address  | tel     |
+----+-----------+-----+------+----------+---------+
|  1 | ZHANG SAN | M   |   18 | BEIJING  | 1234567 |
|  2 | LI SI     | M   |   22 | BEIJING  | 2345678 |
|  3 | WANG WU   | M   |   17 | HUNAN    | 3456789 |
|  4 | ZHAO LIU  | M   |   25 | LIAONING | 4567890 |
+----+-----------+-----+------+----------+---------+
4 rows in set (0.00 sec)
```

6. 删除视图
```
mysql> DROP VIEW info_view;
Query OK, 0 rows affected (0.00 sec)

mysql> DESC info_view;
ERROR 1146 (42S02): Table 'test.info_view' doesn't exist
```