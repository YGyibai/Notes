# 3. 索引

### 3.1 概念

>  索引就像是一本书前面的目录，能加快数据库的查询速度。 它是对数据库表中一列或多列的值进行排序的一种结构，使用索引可快速访问数据库表中的特定信息。 

索引是一个单独存储在磁盘上的数据库结构，它们存储着对数据表里的数据记录的应用指针。

- 其中[MySQL](https://cloud.tencent.com/product/cdb?from=10680)中的索引的存储类型有两种：BTREE、HASH。 也就是用树或者Hash值来存储该字段,具体又和标的存储引擎有关。
- MyISAM 和 InnoDB 存储引擎只支持 BTREE 索引。
- MEMORY / HEAP 存储引擎可以支持 BTREE 和 HASH 索引。

不使用索引的情况下进行检索时，需要遍历和读取整个表，是很耗时的操作。而有了索引后，MySQL 不在全部扫描，直接在索引里找，借助于索引特殊的数据结构（比如 BTREE）可以快速定位这一行数据的位置。

索引的分类： **普通索引和唯一索引**

- 普通索引：是MySQL的基本索引类型，允许重复和空值。
- 唯一索引：值必须是唯一的，可以空值但不能重复。即使是组合索引也必须唯一。
- 主键索引：是一种特殊的唯一索引，不能有空值。

**单列索引和组合索引**

- 单列索引：一个索引仅包含一个列 的索引。
- 组合索引： 由多个字段组合创建的索引。注意在查询条件中使用了左边的字段时，索引才被使用。

**全文索引** 全文索引（ FULLTEXT） ，在创建了全文索引的列上支持值的全文检索。它可以在 CHAR, VARCHAR 或者 TEXT 类型的列上创建。 ***注意：只有 MyISAM 引擎的表才能创建全文索引\***

### 3.2 创建索引

**创建索引的三个方法：**

- 1. 创建表时即创建索引
- 1. 在已存在的表上，使用 “ALTER TABLE” 关键字创建索引
- 1. 在已存在的表上，使用 “CREATE INDEX” 关键字创建索引

#### 3.2.1 创建表时即创建索引

**1、创建普通索引, 在建表时使用关键 “ INDEX ”。示例：**

```mysql
CREATE TABLE book
(
  id                INT(11) NOT NULL PRIMARY KEY AUTO_INCREMENT,
  bookName          VARCHAR(255) NOT NULL ,
  authors           VARCHAR(255) NOT NULL,
  info              VARCHAR(255) NOT NULL,
  comment           VARCHAR(255) NOT NULL,
  year_publication  YEAR NOT NULL,
  INDEX(year_publication)
);
```

**2、创建 唯一索引, 在建表时使用关键 “ UNIQUE INDEX ”。示例：**

```mysql
CREATE TABLE table1 
(
  id  INT NOT NULL,
  name varchar(255) NOT NULL,
  UNIQUE INDEX TheUniqueIdx1(id)
);
```

**3、创建单列索引：**

```mysql
CREATE TABLE table2 
(
  id  INT NOT NULL,
  name varchar(255) NOT NULL,
  INDEX TheSingleIdx1(name(20))
);
#注意，指定了索引长度 20
```

**4、创建组合索引：**

```mysql
CREATE TABLE table5
(
  id  INT NOT NULL,
  name varchar(255) NOT NULL,
  age INT NOT NULL,
  INDEX TheMultiIdx2(id,name,age)
);
```

**5、创建 全文索引，使用关键字 " FULLTEXT INDEX "。只有 MyISAM 存储引擎才支持 全文索引，且仅可以为 CHAR, VARCHAR, 和 TEXT 列创建全文索引。**

```mysql
CREATE TABLE table7
(
  id    INT NOT NULL,
  info  VARCHAR(255) NOT NULL,
  FULLTEXT INDEX TheFulltextIdx(info)
) ENGINE=MyISAM
```

**6、创建空间索引，使用 “ SPATIAL INDEX ” 关键字。它作用于字段类型为 GEOMETRY 上。**

```mysql
CREATE TABLE table8
(
  id    INT NOT NULL,
  poi   GEOMETRY NOT NULL,
  SPATIAL INDEX TheSpatialIdx(poi)
) ENGINE=MyISAM
```

#### 3.2.2  使用 “ALTER TABLE” 关键字在已存在的表上创建索引

和建表时类似，示例：

```mysql
# 普通索引
ALTER TABLE book ADD INDEX TheIdx1(bookName);
# 唯一索引
ALTER TABLE book ADD UNIQUE INDEX TheIdx2(id);
# 组合索引
ALTER TABLE book ADD UNIQUE INDEX TheIdx3(id,authors);
```

#### 3.2.3  使用 “CREATE INDEX” 关键字在已存在的表上创建索引

CREATE INDEX 其实等效于 ALTER TABLE，在 MySQL中 CREATE INDEX  被映射到一个 ALTER TABLE 语句上。示例：

```mysql
# 普通索引
CREATE INDEX BkIndex11 ON book(bookName);
#  唯一索引
CREATE UNIQUE INDEX BkIndex12 ON book(id);
# 组合索引
CREATE UNIQUE INDEX BkIndex13 ON book(authors,info);
```

### 3.3 删除索引

在 MySQL 中可以使用  ALTER TABLE 或者 DROP INDEX 语句来删除一个索引。

两种方法是等效的，DROP INDEX 在内部被映射到一个  ALTER TABLE 上。

```mysql
# 删除一个索引
ALTER TABLE book  DROP INDEX TheIdx1;
# 删除一个索引
DROP INDEX TheIdx2 ON book;
```

### 3.4 扩展知识

**聚簇索引和非聚簇索引** 聚簇索引并不是一种独特的索引类型，而是一种`数据存储方式`。

**即按照索引的存储方式分类：**

- 聚簇索引 (Clustered Index)
- 非聚簇索引  (Non- Clustered Index)，又叫二级索引 （secondary index ）

**简单说就是：**

- 聚簇索引中 索引的顺序就是数据的物理存储顺序；
- 而非聚簇索引的索引顺序与数据物理排列顺序无关。

**InnoDB 引擎是按  B+TREE 结构存储的** InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，聚簇索引就是按照每张表的主键构造一颗B+树，同时叶子节点中存放了`整张表的行记录数据`,也将聚集索引的叶子节点称为数据页。

Innobd中的主键索引是一种聚簇索引，非聚簇索引都是辅助索引，像复合索引、前缀索引、唯一索引。

非聚簇索引（辅助索引) 是在聚簇索引之上创建的索引，辅助索引访问数据总是需要二次查找。辅助索引叶子节点存储的不再是行的物理位置，而是主键值。通过辅助索引首先找到的是主键值，再通过主键值找到数据行的数据页，再通过数据页中的Page Directory找到数据行。

这两种索引内部都是B+树，聚簇索引的叶子节点存放着一整行的数据。而非聚簇索引存放的是主键，要定位到数据记录行 还需要通过主键再到B+树上检索一次。

Innodb使用的是聚簇索引，MyISam使用的是非聚簇索引。

# 4. 扩展

>  EXPLAIN 关键字，用于获取查询执行计划（即 MySQL 如何执行查询的说明。 

EXPLAIN 在对SQL优化分析时很有用，我们可以用 explain 这个命令来查看一个这些SQL语句的执行计划，查看该SQL语句有没有使用上了索引，有没有做全表扫描，这都可以通过explain命令来查看。

比如：

```mysql
EXPLAIN select * from book where year_publication= 1990 \G;

# 执行后：
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: book
   partitions: NULL
         type: ref
possible_keys: year_publication
          key: year_publication
      key_len: 1
          ref: const
         rows: 1
     filtered: 100.00
        Extra: Using index condition
1 row in set, 1 warning (0.00 sec)
```

参考：[https://dev.mysql.com/doc/refman/8.0/en/explain.html](https://links.jianshu.com/go?to=https%3A%2F%2Fdev.mysql.com%2Fdoc%2Frefman%2F8.0%2Fen%2Fexplain.html)

END

