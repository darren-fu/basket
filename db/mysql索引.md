## 概述
mysql的索引分为单列索引(主键索引,唯索引,普通索引)和组合索引.

单列索引:一个索引只包含一个列,一个表可以有多个单列索引.

组合索引:一个组合索引包含两个或两个以上的列

- 普通索引,这个是最基本的索引
``` sql
CREATE INDEX index_name ON table(column(length));
ALTER TABLE table_name ADD INDEX index_name ON (column(length));
CREATE TABLE `table` (
    `id` int(11) NOT NULL AUTO_INCREMENT ,
    `title` char(255) CHARACTER NOT NULL ,
    `content` text CHARACTER NULL ,
    `time` int(10) NULL DEFAULT NULL ,
    PRIMARY KEY (`id`),
    INDEX index_name (title(length))
);
DROP INDEX index_name ON table;
```
> 如果是CHAR,VARCHAR,类型,length可以小于字段的实际长度,如果是BLOB和TEXT类型就必须指定长度

- 唯一索引,与普通索引类似,但是不同的是唯一索引要求所有的类的值是唯一的,这一点和主键索引一样.但是他允许有空值
``` sql
CREATE UNIQUE INDEX indexName ON table(column(length));
ALTER TABLE table_name ADD UNIQUE indexName ON (column(length));
CREATE TABLE `table` (
    `id` int(11) NOT NULL AUTO_INCREMENT ,
    `title` char(255) CHARACTER NOT NULL ,
    `content` text CHARACTER NULL ,
    `time` int(10) NULL DEFAULT NULL ,
    UNIQUE indexName (title(length))
);

```

- 主键索引
``` sql
CREATE TABLE `table` (
    `id` int(11) NOT NULL AUTO_INCREMENT ,
    `title` char(255) NOT NULL ,
    PRIMARY KEY (`id`)
);
```

- 组合索引 指多个字段上创建的索引，只有在查询条件中使用了创建索引时的第一个字段，索引才会被使用。使用组合索引时遵循最左前缀集合
``` sql
ALTER TABLE `table` ADD INDEX name_city_age (name,city,age); 
```
- 全文索引 主要用来查找文本中的关键字，而不是直接与索引中的值相比较
fulltext索引配合match against操作使用，而不是一般的where语句加like。
它可以在create table，alter table ，create index使用，不过目前只有char、varchar，text 列上可以创建全文索引。
值得一提的是，在数据量较大时候，现将数据放入一个没有全局索引的表中，然后再用CREATE index创建fulltext索引，要比先为一张表建立fulltext然后再将数据写入的速度快很多。

``` sql
CREATE TABLE `table` (
    `id` int(11) NOT NULL AUTO_INCREMENT ,
    `title` char(255) CHARACTER NOT NULL ,
    `content` text CHARACTER NULL ,
    `time` int(10) NULL DEFAULT NULL ,
    PRIMARY KEY (`id`),
    FULLTEXT (content)
);
ALTER TABLE article ADD FULLTEXT index_content(content);
CREATE FULLTEXT INDEX index_content ON article(content);

-- 示例
ALTER TABLE tablename ADD FULLTEXT(column1, column2)
ELECT * FROM tablename
WHERE MATCH(column1, column2) AGAINST(‘xxx′, ‘sss′, ‘ddd′)
```

## 索引缺点
- 虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行insert、update和delete。因为更新表时，不仅要保存数据，还要保存一下索引文件。
- 建立索引会占用磁盘空间的索引文件。一般情况这个问题不太严重，但如果你在一个大表上创建了多种组合索引，索引文件的会增长很快。
- 索引只是提高效率的一个因素，如果有大数据量的表，就需要花时间研究建立最优秀的索引，或优化查询语句。

## 索引优点
- 可以通过建立唯一索引或者主键索引,保证数据库表中每一行数据的唯一性.
- 建立索引可以大大提高检索的数据,以及减少表的检索行数
- 在表连接的连接条件 可以加速表与表直接的相连 
- 在分组和排序字句进行数据检索,可以减少查询时间中 分组 和 排序时所消耗的时间(数据库的记录会重新排序)
- 建立索引,在查询中使用索引 可以提高性能

## 注意点
- 在经常需要搜索的列上,可以加快索引的速度
- 主键列上可以确保列的唯一性
- 在表与表的而连接条件上加上索引,可以加快连接查询的速度
- 在经常需要排序(order by),分组(group by)和的distinct 列上加索引 可以加快排序查询的时间,  (单独order by 用不了索引，索引考虑加where 或加limit)
- 在一些where 之后的 < <= > >= BETWEEN IN 以及某个情况下的like 建立字段的索引(B-TREE)

- like语句的 如果你对nickname字段建立了一个索引.当查询的时候的语句是 nickname lick '%ABC%' 那么这个索引讲不会起到作用.而nickname lick 'ABC%' 那么将可以用到索引

- 索引不会包含NULL列,如果列中包含NULL值都将不会被包含在索引中,复合索引中如果有一列含有NULL值那么这个组合索引都将失效,一般需要给默认值0或者 ' '字符串

- 使用短索引,如果你的一个字段是Char(32)或者int(32),在创建索引的时候指定前缀长度 比如前10个字符 (前提是多数值是唯一的..)那么短索引可以提高查询速度,并且可以减少磁盘的空间,也可以减少I/0操作.

- 不要在列上进行运算,这样会使得mysql索引失效,也会进行全表扫描

- 选择越小的数据类型越好,因为通常越小的数据类型通常在磁盘,内存,cpu,缓存中 占用的空间很少,处理起来更快
