如果不适用索引，MYSQL必须从第一条记录开始然后读完整个表直到找出相关的行。索引我是为了快速查询数据库表中的特定记录，提高数据库性能。
> 在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法。这种数据结构，就是索引。

MySQL索引如下：
- B-Tree 索引：最常见的索引类型，大部分引擎都支持B树索引。
- HASH 索引：只有Memory引擎支持，使用场景简单。
- R-Tree 索引(空间索引)：空间索引是MyISAM的一种特殊索引类型，主要用于地理空间数据类型。
- Full-text (全文索引)：全文索引也是MyISAM的一种特殊索引类型，主要用于全文索引，InnoDB从MYSQL5.6版本提供对全文索引的支持。

MySQL有如下的索引：

| 索引 | MyISAM引擎 | InnoDB引擎 | Memory引擎 |
|------|:----------|:-----------|:-----------|
|B—Tree |支持|支持|支持|
| HASH |不支持|不支持|支持|
|R-Tree|支持|不支持|不支持
|Full-text|不支持|暂不支持|不支持|


B-TREE索引类型

* 普通索引
这是最基本的索引类型，而且它没有唯一性之类的限制。普通索引可以通过以下几种方式创建：
（1）创建索引: CREATE INDEX 索引名 ON 表名(列名1，列名2,...);
（2）修改表: ALTER TABLE 表名ADD INDEX 索引名 (列名1，列名2,...);
（3）创建表时指定索引：CREATE TABLE 表名 ( [...], INDEX 索引名 (列名1，列名 2,...) );
* UNIQUE索引
表示唯一的，不允许重复的索引，如果该字段信息保证不会重复例如身份证号用作索引时，可设置为unique：
（1）创建索引：CREATE UNIQUE INDEX 索引名 ON 表名(列的列表);
（2）修改表：ALTER TABLE 表名ADD UNIQUE 索引名 (列的列表);
（3）创建表时指定索引：CREATE TABLE 表名( [...], UNIQUE 索引名 (列的列表) );
* 主键：PRIMARY KEY索引
主键是一种唯一性索引，但它必须指定为“PRIMARY KEY”。
（1）主键一般在创建表的时候指定：“CREATE TABLE 表名( [...], PRIMARY KEY (列的列表) ); ”。
（2）但是，我们也可以通过修改表的方式加入主键：“ALTER TABLE 表名ADD PRIMARY KEY (列的列表); ”。
每个表只能有一个主键。 （主键相当于聚合索引，是查找最快的索引）
注：不能用CREATE INDEX语句创建PRIMARY KEY索引

索引的设置语法

#### 一 设置索引
在执行CREATE TABLE语句时可以创建索引，也可以单独用CREATE INDEX或ALTER TABLE来为表增加索引。

1．ALTER TABLE - ALTER TABLE用来创建普通索引、UNIQUE索引或PRIMARY KEY索引。
```
ALTER TABLE table_name ADD INDEX index_name (column_list)
ALTER TABLE table_name ADD UNIQUE (column_list)
ALTER TABLE table_name ADD PRIMARY KEY (column_list)
```
2．CREATE INDEX - CREATE INDEX可对表增加普通索引或UNIQUE索引。
```
CREATE INDEX index_name ON table_name (column_list)
CREATE UNIQUE INDEX index_name ON table_name (column_list)
```
#### 二 删除索引
可利用ALTER TABLE或DROP INDEX语句来删除索引。类似于CREATE INDEX语句，DROP INDEX可以在ALTER TABLE内部作为一条语句处理，语法如下。
```
DROP INDEX index_name ON talbe_name
ALTER TABLE table_name DROP INDEX index_name
ALTER TABLE table_name DROP PRIMARY KEY
```
其中，前两条语句是等价的，删除掉table_name中的索引index_name。
第3条语句只在删除PRIMARY KEY索引时使用，因为一个表只可能有一个PRIMARY KEY索引，因此不需要指定索引名。如果没有创建PRIMARY KEY索引，但表具有一个或多个UNIQUE索引，则MySQL将删除第一个UNIQUE索引。

> 如果从表中删除了某列，则索引会受到影响。对于多列组合的索引，如果删除其中的某列，则该列也会从索引中删除。如果删除组成索引的所有列，则整个索引将被删除。

#### 三 查看索引

```
mysql> show index from tblname;
mysql> SHOW KEYS FROM tblname;
```
- Table：表的名称
- Non_unique：如果索引不能包括重复词，则为0。如果可以，则为1
- Key_name：索引的名称
- Seq_in_index：索引中的列序列号，从1开始
- Column_name：列名称
- Collation：列以什么方式存储在索引中。在MySQL中，有值‘A’（升序）或NULL（无分类）。
- Cardinality：索引中唯一值的数目的估计值。通过运行ANALYZE TABLE或myisamchk -a可以更新。基数根据被存储为整数的统计数据来计数，所以即使对于小型表，该值也没有必要是精确的。基数越大，当进行联合时，MySQL使用该索引的机会就越大。
- Sub_part：如果列只是被部分地编入索引，则为被编入索引的字符的数目。如果整列被编入索引，则为NULL。
- Packed：指示关键字如何被压缩。如果没有被压缩，则为NULL。
- Null：如果列含有NULL，则含有YES。如果没有，则该列含有NO。
- Index_type：用过的索引方法（BTREE, FULLTEXT, HASH, RTREE）。
- Comment：更多评注。

#### 索引选择性

#### 一 索引选择原则

1. 较频繁的作为查询条件的字段应该创建索引
2. 唯一性太差的字段不适合单独创建索引，即使频繁作为查询条件
3. 更新非常频繁的字段不适合创建索引
4. 不会出现在 WHERE 子句中的字段不该创建索引

#### 二 索引选择原则细述
- 性能优化过程中，选择在哪个列上创建索引是最非常重要的。可以考虑使用索引的主要有 两种类型的列：在where子句中出现的列，在join子句中出现的列，而不是在SELECT关键字后选择列表的列；
- 索引列的基数越大，索引的效果越好。例如，存放出生日期的列具有不同的值，很容易区分行，而用来记录性别的列，只有"M"和"F",则对此进行索引没有多大用处，因此不管搜索哪个值，都会得出大约一半的行,（ 见索引选择性注意事项对选择性解释；）
使用短索引，如果对字符串列进行索引，应该指定一个前缀长度，可节省大量索引空间，提升查询速度；
>例如，有一个CHAR(200)列，如果在前10个或20个字符内，多数值是唯一的，那么就不要对整个列进行索引。对前10个或者20个字符进行索引能够节省大量索引空间，也可能会使查询更快。较小的索引涉及的磁盘IO较少，较短的值比较起来更快。更为重要的是，对于较短的键值，所以高速缓存中的快能容纳更多的键值，因此，MYSQL也可以在内存中容纳更多的值。这样就增加了找到行而不用读取索引中较多快的可能性。

- 利用最左前缀

#### 三 索引选择注意事项
既然索引可以加快查询速度，那么是不是只要是查询语句需要，就建上索引？答案是否定的。因为索引虽然加快了查询速度，但索引也是有代价的：索引文件本身要消耗存储空间，同时索引会加重插入、删除和修改记录时的负担，另外，MySQL在运行时也要消耗资源维护索引，因此索引并不是越多越好。

一般两种情况下不建议建索引：

- 表记录比较少，例如一两千条甚至只有几百条记录的表，没必要建索引，让查询做全表扫描就好了;
> 至于多少条记录才算多，这个个人有个人的看法，我个人的经验是以2000作为分界线，记录数不超过 2000可以考虑不建索引，超过2000条可以酌情考虑索引。
索引的选择性较低。所谓索引的选择性（Selectivity），是指不重复的索引值（也叫基数，Cardinality）与表记录数（#T）的比值：
Index Selectivity = Cardinality / #T
显然选择性的取值范围为(0, 1]，选择性越高的索引价值越大，这是由B+Tree的性质决定的。例如，上文用到的employees.titles表，如果title字段经常被单独查询，是否需要建索引，我们看一下它的选择性：
SELECT count(DISTINCT(title))/count(*) AS Selectivity FROM employees.titles;
+-------------+
| Selectivity |
+-------------+
|      0.0000 |
+-------------+
title的选择性不足0.0001（精确值为0.00001579），所以实在没有什么必要为其单独建索引。
MySQL只对一下操作符才使用索引：<,<=,=,>,>=,between,in, 以及某些时候的like(不以通配符%或_开头的情形)。

- 不要过度索引，只保持所需的索引。每个额外的索引都要占用额外的磁盘空间，并降低写操作的性能。 在修改表的内容时，索引必须进行更新，有时可能需要重构，因此，索引越多，所花的时间越长。
 
#### 四 索引的弊端
索引的益处已经清楚了，但是我们不能只看到这些益处，并认为索引是解决查询优化的圣经，只要发现 查询运行不够快就将 WHERE 子句中的条件全部放在索引中。

确实，索引能够极大地提高数据检索效率，也能够改善排序分组操作的性能，但有不能忽略的一个问题就是索引是完全独立于基础数据之外的一部分数据。假设在Table ta 中的Column ca 创建了索引 idx_ta_ca，那么任何更新 Column ca 的操作，MySQL在更新表中 Column ca的同时，都须要更新Column ca 的索引数据，调整因为更新带来键值变化的索引信息。而如果没有对 Column ca 进行索引，MySQL要做的仅仅是更新表中 Column ca 的信息。这样，最明显的资源消耗就是增加了更新所带来的 IO 量和调整索引所致的计算量。此外，Column ca 的索引idx_ta_ca须要占用存储空间，而且随着 Table ta 数据量的增加，idx_ta_ca 所占用的空间也会不断增加，所以索引还会带来存储空间资源消耗的增加。

[1] https://segmentfault.com/a/1190000003072424