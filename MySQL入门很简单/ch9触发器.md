
触发器是由事件来出发某个操作，这些事件包括INSERT语句、UPDATE语句、DELETE语句。

## 创建

```
CREATE TRIGGER trigger_name
trigger_time
trigger_event ON tbl_name
FOR EACH ROW
trigger_stmt
```

- trigger_name：标识触发器名称，用户自行指定；
- trigger_time：标识触发时机，取值为 BEFORE 或 AFTER；
- trigger_event：标识触发事件，取值为 INSERT、UPDATE 或 DELETE；
- tbl_name：标识建立触发器的表名，即在哪张表上建立触发器；
- trigger_stmt：触发器程序体，可以是一句SQL语句，或者用 BEGIN 和 END 包含的多条语句。

**注意：**一个表上不能建立两个相同类型的触发器。

## 查看

```
SHOW TRIGGERS [FROM schema_name];
```
其中，schema_name 即 Schema 的名称，在 MySQL 中 Schema 和 Database 是一样的，也就是说，可以指定数据库名，这样就不必先“USE database_name;”了。

## 删除

```
DROP TRIGGER [IF EXISTS] [schema_name.]trigger_name
```

## 例子

```
mysql> create table product(
    -> id int(10) not null unique primary key,
    -> name varchar(20) not null,
    -> function varchar(50),
    -> company varchar(20) not null,
    ->  address varchar(50));
Query OK, 0 rows affected (0.66 sec)

mysql> create table operate(
    -> op_id int(10) not null primary key auto_increment,
    -> op_name varchar(20) not null,
    -> op_time time not null);
Query OK, 0 rows affected (0.42 sec)

```

```
mysql> create trigger product_bf_insert before insert
    -> on product for each row
    -> insert into operate values(null, 'insert product', now());
Query OK, 0 rows affected (0.41 sec)

mysql>  create trigger product_af_update after update
    ->  on product for each row
    ->  insert into operate value(null,'update product',now());
Query OK, 0 rows affected (0.70 sec)

mysql> create trigger product_af_del after delete
    -> on product for each row
    -> insert into operate value(null,'delete product',now());
Query OK, 0 rows affected (1.15 sec)

```

```
mysql> insert into product values(1, 'baijiahei', 'zhiganmao', 'beijingzhiyaocha
ng','beijingcangpingqu');
Query OK, 1 row affected (3.33 sec)

mysql> select * from operate;
+-------+----------------+----------+
| op_id | op_name        | op_time  |
+-------+----------------+----------+
|     1 | insert product | 16:08:30 |
+-------+----------------+----------+
1 row in set (0.63 sec)

mysql> update product set address='beijinghaidianqu' where id=1;
Query OK, 1 row affected (0.33 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from operate;
+-------+----------------+----------+
| op_id | op_name        | op_time  |
+-------+----------------+----------+
|     1 | insert product | 16:08:30 |
|     2 | update product | 16:09:39 |
+-------+----------------+----------+
2 rows in set (0.00 sec)

mysql> delete from product where id=1;
Query OK, 1 row affected (0.59 sec)

mysql> select * from operate;
+-------+----------------+----------+
| op_id | op_name        | op_time  |
+-------+----------------+----------+
|     1 | insert product | 16:08:30 |
|     2 | update product | 16:09:39 |
|     3 | delete product | 16:10:11 |
+-------+----------------+----------+
3 rows in set (0.00 sec)

mysql> drop trigger product_bf_insert;
Query OK, 0 rows affected (0.06 sec)

mysql> select * from information_schema.triggers where trigger_name='product_bf_
insert' \G
Empty set (0.02 sec)
```