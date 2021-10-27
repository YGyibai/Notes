# MySQL学习笔记(3) - 表的基本操作

2021-06-24阅读 660

###1. 背景

本文讲表的基本操作。

###2.知识

>  在[数据库](https://cloud.tencent.com/solution/database?from=10680)中，数据表是基本的操作对象，是数据存储的基本单位。数据表被定义为列的集合，数据表是按行和列的格式来存储的。每行代表唯一的一条数据记录，每列代表记录中的对象的一个属性。 

###3. 示例

**(1) 新建表**

```mysql
CREATE TABLE tb_table1
(
  id INT(11),
  name VARCHAR(25),
  deptId INT(11),
  salary FLOAT
);
```

**(2) 查看已经有哪些表**

```mysql
show tables;
```

**(3) 主键约束，外键约束，非空约束，唯一约束，默认值约束**

- 主键 能够唯一地标识表中的一条记录，就像是身份证。可以是单个字段做主键，也可以多字段做联合主键。
- 外键 用来在两个表的数据之间建立连接。它一般对应另外一个表的主键。外键的作用是保证数据引用的完整性。一个表的外键可以是空值，如果不为空则必须是某个表中主键的值。
- 非空约束：使用NOT NULL 指定字段的值不能为空
- 唯一性约束 用于说明该列的值必须是唯一的，可以为空但不能重复。
- 主键约束和唯一约束的区别：一个表中只能有一个主键，可以有多个唯一键。主键不能有空值，而唯一键可以有空值。
- 默认约束 用来指定某列的默认值，比如 一个数字型的列默认0，在插入表时可以不指定具体值，默认插入0到该列中。
- 自增： 使用 AUTO_INCREMENT关键字声明自增列，从1开始，每次新增一条记录，字段值自动加1。一个表只能有一个 自增列。

示例：

```mysql
CREATE TABLE tb_table2
(
  id INT(11) NOT NULL PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(25) NULL UNIQUE,
  deptId INT(11) NULL DEFAULT 0,
  salary FLOAT
);
```

**(4) 查看表结构** 使用 DESCRIBE 或者 DESC 查看表的字段信息。

```mysql
DESCRIBE tb_table3;
或者：
DESC tb_table3;
```

使用 SHOW CREATE TABLE 语句可以用来显示创建表时的 SQL 语句。示例：

```mysql
show create table tb_table1;
或者： 
show create table tb_table1 \G;
```

**(5) 修改数据表** 修改数据表是指 通过 ALTER TABLE 语句修改库中的表的结构，常用的操作有：

- 修改表名
- 修改字段类型或字段名称
- 增加和删除字段
- 修改字段的排列位置
- 更改表的存储引擎
- 删除外键约束等

示例：

```mysql
# 修改表名： 
ALTER TABLE tb_table1 RENAME tb_table3;
# 修改字段类型
ALTER TABLE tb_table3 MODIFY name varchar(50);
# 修改字段名称
ALTER TABLE tb_table3 CHANGE deptId dept int;
# 添加新字段
ALTER TABLE tb_table3 ADD shotName varchar(50);
# 在name后添加一个字段
ALTER TABLE tb_table3 ADD nicktName varchar(50) AFTER name;
# 删除一个字段
ALTER TABLE tb_table3 DROP shotName;
# 修改表的引擎
ALTER TABLE tb_table3 ENGINE=MyISAM;
```

**(6) 删除表** 使用 DROP TABLE 可以删除一个或者多个表。

```mysql
DROP TABLE tb_table3;
```

END

