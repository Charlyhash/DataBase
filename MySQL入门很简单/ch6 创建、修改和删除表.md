表是数据存储数据的基本单位，一个表包含若干个字段或记录。

* 创建表的方法
* 表的完整性约束条件
* 修改表的方法
* 删除表的方法

## 创建表

创建表是在已经存在的数据库中建立新表。

#### 创建表的语法形式

```
CREATE TABLE 表名(属性名 数据类型 [完整性约束条件]，
                 属性名 数据类型 [完整性约束条件]，
                   ...
                   属性名 数据类型
                    );

```

* 表名：创建的表的名字
* 属性名：表中字段的名字
* 数据类型：字段的类型
* 完整性约束条件：指定字段的某些特殊约束条件

表名不能是sql关键字，一个表有一个或多个属性，各个属性之间用逗号隔开，最后一个不加逗号。

完整性约束条件是对字段的限制，要求用户对该属性进行操作符合特定的要求。有如下几种：
* PRIMARY KEY ：主键
* FOREIGN KEY：外键
* NOT NULL：非空
* UNIQUE：唯一
* AUTO_INCREMENT：自动增加
* DEFAULT:默认值

#### 设置表的主键

主键是表的特殊字段，唯一标识该表的每条信息，每条记录的主键都不同，且非空。可以是单个字段，也可以是多个字段的组合。

* 单字段主键：在该字段后面直接加上PRIMARY KYE
* 多字段主键：在属性定义完成之后，统一设置主键。PRIMARY KEY(属性名1， 属性名2， ...);

#### 设置表的外键

比如：字段sno是表A的属性，且依赖于表B的主键。那么B为父表，A为子表，sno是表A的外键。
创建外键的原则是必须依赖数据库中已经存在的父表的主键，外键可以为空。
外键作用是为了建立与父表的联系，当父表中删除了某条信息时，子表中对应的信息也要改变。
设置外键的语法：
```
CONSTRAINT 外键别名 FOREIGN KEY(属性1.1，属性1.2,...属性1.n)
    REFERENCES 表名(属性2.1, 属性2.2, ... , 属性2.n)
```
* 外键别名：外键的代号
* 属性1，子表中设置的外键
* 表名：父表的名字
* 属性2：父表的主键


#### 设置表的非空约束：NOT NULL
#### 设置表的唯一性约束：UNIQUE
#### 设置表的自动增加： AUTO_INCREMENT
#### 设置表的属性的默认值：DEFAULT 默认值

## 查看表的结构

#### 查看表结构基本语法
DESC 表名

#### 查看表详细结构语法

```
mysql> CREATE TABLE grade(
    -> id INT(10) NOT NULL UNIQUE PRIMARY KEY AUTO_INCREMENT,
    -> course VARCHAR(10) NOT NULL,
    -> s_num INT(10) NOT NULL,
    -> grade VARCHAR(4),
    -> CONSTRAINT grade_fk FOREIGN KEY (s_num)
    -> REFERENCES student(num)
    -> );
Query OK, 0 rows affected (1.18 sec)

mysql> SHOW CREATE TABLE grade \G
*************************** 1. row ***************************
       Table: grade
Create Table: CREATE TABLE `grade` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `course` varchar(10) NOT NULL,
  `s_num` int(10) NOT NULL,
  `grade` varchar(4) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `id` (`id`),
  KEY `grade_fk` (`s_num`),
  CONSTRAINT `grade_fk` FOREIGN KEY (`s_num`) REFERENCES `student` (`num`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
1 row in set (0.00 sec)
```

## 修改表

#### 修改表名

#### 修改字段的数据类型

```
ALTER TABLE (表名) MODIFY (字段名) (新数据类型);
```

#### 修改字段名

#### 增加字段

#### 删除字段

#### 修改字段的排列位置

#### 更该表的存储引擎

#### 删除表的外键约束

## 删除表

#### 删除没有关联的普通表

#### 删除被其他表关联的父表




## 实例：

* 查询并使用student数据库

```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| first_database     |
| mysql              |
| performance_schema |
| student            |
| test               |
+--------------------+
6 rows in set (0.07 sec)

mysql> USE STUDENT
Database changed
```

* 创建表

```
mysql>  CREATE TABLE student(num INT(10) NOT NULL UNIQUE PRIMARY KEY,
    -> name VARCHAR(20) NOT NULL,
    -> sex VARCHAR(4) NOT NULL,
    -> birthday DATETIME,
    -> address VARCHAR(50)
    -> );
Query OK, 0 rows affected (1.07 sec)
```
```
mysql> DESC student
    -> ;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| num      | int(10)     | NO   | PRI | NULL    |       |
| name     | varchar(20) | NO   |     | NULL    |       |
| sex      | varchar(4)  | NO   |     | NULL    |       |
| birthday | datetime    | YES  |     | NULL    |       |
| address  | varchar(50) | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
5 rows in set (0.03 sec)
```

创建 grade表

```
mysql> CREATE TABLE grade(
    -> id INT(10) NOT NULL UNIQUE PRIMARY KEY AUTO_INCREMENT,
    -> course VARCHAR(10) NOT NULL,
    -> s_num INT(10) NOT NULL,
    -> grade VARCHAR(4),
    -> CONSTRAINT grade_fk FOREIGN KEY (s_num)
    -> REFERENCES student(num)
    -> );
Query OK, 0 rows affected (1.18 sec)


mysql> SHOW CREATE TABLE grade \G
*************************** 1. row ***************************
       Table: grade
Create Table: CREATE TABLE `grade` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `course` varchar(10) NOT NULL,
  `s_num` int(10) NOT NULL,
  `grade` varchar(4) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `id` (`id`),
  KEY `grade_fk` (`s_num`),
  CONSTRAINT `grade_fk` FOREIGN KEY (`s_num`) REFERENCES `student` (`num`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
1 row in set (0.00 sec)
```

* 修改字段数据类型

```
ALTER TABLE grade MODIFY course VARCHAR(20);

mysql> DESC grade;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int(10)     | NO   | PRI | NULL    | auto_increment |
| course | varchar(20) | YES  |     | NULL    |                |
| s_num  | int(10)     | NO   | MUL | NULL    |                |
| grade  | varchar(4)  | YES  |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
4 rows in set (0.01 sec)
```

* 将s_sum字段的位置改到course字段的前面

```
mysql> ALTER TABLE grade MODIFY s_num INT(10) AFTER id;
Query OK, 0 rows affected (1.17 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC grade;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int(10)     | NO   | PRI | NULL    | auto_increment |
| s_num  | int(10)     | YES  | MUL | NULL    |                |
| course | varchar(20) | YES  |     | NULL    |                |
| grade  | varchar(4)  | YES  |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
4 rows in set (0.01 sec)
```

* 将grade字段改为score

```
mysql> ALTER TABLE grade CHANGE grade score VARCHAR(4);
Query OK, 0 rows affected (0.51 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC grade;
+--------+-------------+------+-----+---------+----------------+
| Field  | Type        | Null | Key | Default | Extra          |
+--------+-------------+------+-----+---------+----------------+
| id     | int(10)     | NO   | PRI | NULL    | auto_increment |
| s_num  | int(10)     | YES  | MUL | NULL    |                |
| course | varchar(20) | YES  |     | NULL    |                |
| score  | varchar(4)  | YES  |     | NULL    |                |
+--------+-------------+------+-----+---------+----------------+
4 rows in set (0.00 sec)
```

* 删除外键。在没有删除时，不能修改存储引擎。

```
mysql> ALTER TABLE grade ENGINE=MyISAM;
ERROR 1217 (23000): Cannot delete or update a parent row: a foreign key constrai
nt fails

mysql> ALTER TABLE grade DROP FOREIGN KEY grade_fk;
Query OK, 0 rows affected (0.50 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> SHOW CREATE TABLE grade \G
*************************** 1. row ***************************
       Table: grade
Create Table: CREATE TABLE `grade` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `s_num` int(10) DEFAULT NULL,
  `course` varchar(20) DEFAULT NULL,
  `score` varchar(4) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `id` (`id`),
  KEY `grade_fk` (`s_num`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
1 row in set (0.00 sec)
```

* 更换存储引擎

```
mysql> ALTER TABLE grade ENGINE=MyISAM;
Query OK, 0 rows affected (0.42 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> SHOW CREATE TABLE grade \G
*************************** 1. row ***************************
       Table: grade
Create Table: CREATE TABLE `grade` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `s_num` int(10) DEFAULT NULL,
  `course` varchar(20) DEFAULT NULL,
  `score` varchar(4) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `id` (`id`),
  KEY `grade_fk` (`s_num`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8
1 row in set (0.00 sec)
```

* 删除一个字段

```
mysql> ALTER TABLE student DROP address;
Query OK, 0 rows affected (0.42 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC  student;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| num      | int(10)     | NO   | PRI | NULL    |       |
| name     | varchar(20) | NO   |     | NULL    |       |
| sex      | varchar(4)  | NO   |     | NULL    |       |
| birthday | datetime    | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

* 增加字段

```
mysql> ALTER TABLE student ADD phone INT(10);
Query OK, 0 rows affected (0.35 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> DESC  student;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| num      | int(10)     | NO   | PRI | NULL    |       |
| name     | varchar(20) | NO   |     | NULL    |       |
| sex      | varchar(4)  | NO   |     | NULL    |       |
| birthday | datetime    | YES  |     | NULL    |       |
| phone    | int(10)     | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
5 rows in set (0.01 sec)
```

* 修改表名

```
mysql> ALTER TABLE grade RENAME gradeInfo;
Query OK, 0 rows affected (0.03 sec)

mysql> SHOW TABLES;
+-------------------+
| Tables_in_student |
+-------------------+
| gradeinfo         |
| student           |
+-------------------+
2 rows in set (0.00 sec)
```

* 删除表

```
mysql> DROP TABLE student;
Query OK, 0 rows affected (0.10 sec)

mysql> SHOW TABLES;
+-------------------+
| Tables_in_student |
+-------------------+
| gradeinfo         |
+-------------------+
1 row in set (0.00 sec)
```