#  MySQL

[TOC]

## 理论知识

### 基本概念

#### DB

数据库，database，存储数据的仓库

#### DBMS

数据库管理系统，database management system，数据库是通过DBMS创建和操作的容器。常见的有MySQL，oracle，gbase，sqlserver等

#### SQL

结构化查询语言，structure query language，专门用来与数据库通信的语言
sql的优点：

- 不是某个特定数据库供应商的专有语言，技术所有DBMS都支持sql，只不过在细节上可能不一样
- 简单易学
- 可以灵活使用进行复杂和高级的数据库操作

### 存储特点

- 将数据放到表中，表在放到库中
- 一个库可以有多张表，每个表的名字具有唯一性
- 表中有一些特性，这些特性定义了数据在表中如何存储
- 表由列构成，也称为“字段”，所有表都有若干个列组成，每一列类似于Java中的一个成员变量
- 表中数据按行存储，每一行相当于一个对象

### 配置文件

my.ini

## 操作命令

### 数据库连接和退出

- ```shell
  mysql -h 主机名 -P 端口号 -u 用户名 -p
  ```

- `exit`  退出

### 查看版本号

- 进入mysql的情况下：select version();
- 不进入mysql：mysql --version或mysql -V

## SQL语句

==SQL 是一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统。==与其他程序设计语言（如 C语言、Java 等）不同的是，SQL 由很少的关键字组成，每个 SQL 语句通过一个或多个关键字构成。

SQL包含以下四部分：

#### 1）数据定义语言（Data Definition Language，DDL）

用来创建或删除数据库以及表等对象，主要包含以下几种命令：

- DROP：删除数据库和表等对象
- CREATE：创建数据库和表等对象
- ALTER：修改数据库和表等对象的结构

#### 2）数据操作语言（Data Manipulation Language，DML）

用来变更表中的记录，主要包含以下几种命令：

- SELECT：查询表中的数据
- INSERT：向表中插入新数据
- UPDATE：更新表中的数据
- DELETE：删除表中的数据

#### 3）数据查询语言（Data Query Language，DQL）

用来查询表中的记录，主要包含 SELECT 命令，来查询表中的数据。

#### 4）数据控制语言（Data Control Language，DCL）

用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对数据库中的用户设定权限。主要包含以下几种命令：

- GRANT：赋予用户操作权限
- REVOKE：取消用户的操作权限
- COMMIT：确认对数据库中的数据进行的变更
- ROLLBACK：取消对数据库中的数据进行的变更

#### 语法规范

- 不区分大小写，但建议关键字大写，表名，列名小写

- 每条命令分号结尾

- 根据需要，命令可以换行缩进等

-  注释
   - 单行：#注释文字
   - 单行：-- 注释文字
   - 多行：/* 注释文字 */

## 数据库操作

#### 查看数据库

`show databases [like 数据库名]`;

模糊查询：%

- information_schema：主要存储了系统中的一些数据库对象信息，比如用户表信息、列信息、权限信息、字符集信息和分区信息等。
- mysql：MySQL 的核心数据库，类似于 SQL Server 中的 master 表，主要负责存储数据库用户、用户访问权限等 MySQL 自己需要使用的控制和管理信息。常用的比如在 mysql 数据库的 user 表中修改 root 用户密码。
- performance_schema：主要用于收集数据库服务器性能参数。
- sakila：MySQL 提供的样例数据库，该数据库共有 16 张表，这些数据表都是比较常见的，在设计数据库时，可以参照这些样例数据表来快速完成所需的数据表。
- sys：MySQL 5.7 安装完成后会多一个 sys 数据库。sys 数据库主要提供了一些视图，数据都来自于 performation_schema，主要是让开发者和使用者更方便地查看性能问题。
- world：world 数据库是 MySQL 自动创建的数据库，该数据库中只包括 3 张数据表，分别保存城市，国家和国家使用的语言等内容。

#### 创建数据库

```sql
create database [if not exists] 数据库名
[[default] character set <字符集名>] 
[[default] collate <校对规则名>];
```
`CREATE DATABASE lmn CHARACTER SET 'UTF8';`

- <数据库名>：创建数据库的名称。MySQL 的数据存储区将以目录方式表示 MySQL 数据库，因此数据库名称必须符合操作系统的文件夹命名规则，==不能以数字开头==，尽量要有实际意义。注意在 MySQL 中不区分大小写。
- IF NOT EXISTS：在创建数据库之前进行判断，只有该数据库目前尚不存在时才能执行操作。此选项可以用来避免数据库已经存在而重复创建的错误。
- [DEFAULT] CHARACTER SET：指定数据库的字符集。指定字符集的目的是为了避免在数据库中存储的数据出现乱码的情况。如果在创建数据库时不指定字符集，那么就使用系统的默认字符集。
- [DEFAULT] COLLATE：指定字符集的默认校对规则。

#### 修改数据库

```sql
alter database [数据库名] { 
[default] character set <字符集名> |
[default] collate <校对规则名>};
```



#### 删除数据库

`drop database [if exists] 数据库名;`

==MySQL 安装后，系统会自动创建名为 information_schema 和 mysql 的两个系统数据库，系统数据库存放一些和数据库相关的信息，如果删除了这两个数据库，MySQL 将不能正常工作。==

#### 选择数据库

`use 数据库名` 
`use xsh`		选择xsh数据库

#### 查询当前所在库

`select database();`

### 数据表操作

#### 创建表

```sql
create table [if not exists] 表名(
	字段1 数据类型[字段属性 | 约束] [索引][注释]，
	……
	字段n 数据类型[字段属性 | 约束] [索引][注释]
)[表类型][表字符集][注释];
```

- <表名>：指定要创建表的名称，在 CREATE TABLE 之后给出，必须符合标识符命名规则。==表名称被指定为 db_name.tbl_name，以便在特定的数据库中创建表。无论是否有当前数据库，都可以通过这种方式创建==。在当前数据库中创建表时，可以省略 db-name。如果使用加引号的识别名，则应对数据库和表名称分别加引号。例如，'mydb'.'mytbl' 是合法的，但 'mydb.mytbl' 不合法。

  

以下面表格为例，用命令创建表

| 字段名称 | 数据类型 | 备注   |
| -------- | -------- | ------ |
| id       | int      | 编号   |
| name     | varchar  | 姓名   |
| sex      | varchar  | 性别   |
| phone    | varchar  | 手机号 |
```sql
 CREATE TABLE `ss` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `sex` varchar(255) DEFAULT NULL,
  `phone` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

#### 查看表结构

`describe 表名`

或

`desc 表名`

以sql语句的形式展示表结构

`show create table 表名`

#### 修改表

`alter table 表名[修改选项]`

修改选项语法：

```sql
{ ADD COLUMN <列名> <类型>
| CHANGE COLUMN <旧列名> <新列名> <新列类型>
| ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT }
| MODIFY COLUMN <列名> <类型>
| DROP COLUMN <列名>
| RENAME TO <新表名>
| CHARACTER SET <字符集名>
| COLLATE <校对规则名> }
```

练习：

1. 修改列名
2. 修改列的类型或约束
3. 添加新列
4. 删除列
5. 修改表名

*注意*： 

​	first添加到首行

​	after 列名 添加到某一行之后

​	添加时类型、约束等的写法和创建一样

#### 删除表



## 数据操作
 ### 插入
 insert into 表名（列名，列名）values （xxx，xxx）
实例
`insert into user_table (userid,username) values (null,'Logan')`
**可以插入多行，可以支持子查询**
实例
`insert into user_table (userid,username) values (null,'Logan'),(null,'Joker')`

**方式二（不常见）**

` insert into 表名 set 列名=值，列名= 值....`
实例

`insert into user_table set userid= null,username='xiong'`

### 修改

`update 表名 set 列=值，列=值 where 筛选条件 `

### 删除

**删除单行**
`delete from 表名 where 筛选条件`
实例
`delete from user_table where userName='Logan'`

**删除表中所有数据**
`truncate table 表名`
实例
`truncate table user_table`

### 查询

`select 字段，字段 from 表名 where 筛选条件`



**条件运算符：**> 		 <  		=  		!=    	<>(MySQL默认不等于是<>)  	>= 	<=

`SELECT userName from user_table where userid >5`

**逻辑运算符：** and		or		not() 非 

`SELECT userName,sex from user_table where userid >5 AND sex='男'`

`SELECT userName,sex from user_table where userid >5 and  NOT(sex='n')`

**模糊查询：** like（%表示任意多个字符，_表示单个字符），between and ，in， is null。
				**like**

`select * FROM user_table WHERE userName like 'J_ker'`

`select * FROM user_table WHERE userName like 'J%'`

​				**between and**  

​	1.查询某段范围内的数据

`select * FROM user_table where userid BETWEEN 10 AND 19`

查询所有当userid在10到19之间的所有数据

​	2.BETWEEN 还具有数据比较功能
​	语法如下 `expr BETWEEN min AND max`

​	当 expr 表达式的值大于或等于 min 且小于或等于 max 时, BETWEEN 的返回值为 1 ，否则返回 0 。利用这个功能，可以判断一个表达式或值否则在某个区间：
`SELECT 1 BETWEEN 2 AND 3` 返回0	
​			
​			**in**
`select * from test where  id in (1,2,3,4)`
等同于 	`select  * from test  where (id=1 or id=2 or id=3 or id=4)`

当 IN 前面加上 NOT 运算符时，表示与 IN 相反的意思，即不在这些列表项内选择。

`WHERE column NOT IN (列表项)`

**别名**

As  空格

去重 distinct

拼接 concat

**排序 order by**
asc 升序
des 降序
**分组查询 group by**

查询前操作

**having**

查询后对数据进行操作

### MySQL 聚合函数

| 函数名称                                         | 作用                             |
| ------------------------------------------------ | -------------------------------- |
| [MAX](http://c.biancheng.net/mysql/max.html)     | 查询指定列的最大值               |
| [MIN](http://c.biancheng.net/mysql/min.html)     | 查询指定列的最小值               |
| [COUNT](http://c.biancheng.net/mysql/count.html) | 统计查询结果的行数               |
| [SUM](http://c.biancheng.net/mysql/sum.html)     | 求和，返回指定列的总和           |
| [AVG](http://c.biancheng.net/mysql/avg.html)     | 求平均值，返回指定列数据的平均值 |

```mysql
#查询每个班有多少人
select count(*) 
from student_table 
group by classid;

#查询每个班的最大年龄是多少
select max(age),classid 
from student_table
group by classid;

#查询每个班学生年龄最大值、最小值、平均值、总和，并按班级降序
#知识点 ：max min avg sum group by 分组 order by desc降序，别名
select max(age) 最大值,min(age) 最小值, avg(age) 平均值, sum(age) 总和, classid
from student_table
group by classid
order by classid desc;
```

### 多表查询 

#### SQL92

**等值查询（92）**92年指定的sql语句规则

```
select 列名 
from 表1，表2
where 筛选条件 【group by 分组】【having 筛选条件】【order by 排序列表】
```

**非等值查询（92）**

```
select 列名 
from 表1，表2
where 筛选条件 【group by 分组】【having 筛选条件】【order by 排序列表】
```

筛选条件为一个范围

**自连接：**

自己连接自己

#### **SQL（99）**99年指定的SQL语法规则

```
select 列名
from 表1 别名【连接类型】
join 表2 别名
on 连接条件
【where 筛选条件】
【group by 分组】
【having 筛选条件】
【order by 排序列表】
```

**1.内连接查询（只显示符合条件的数据，相当于两张表的交集）**

```mysql
#查询人员和部门所有信息
select * from person inner join dept on person.did =dept.did;
```

**2.左外连接查询（左边表中的数据优先全部显示）**

```mysql
#查询人员和部门所有信息
select * from person left join dept on person.did =dept.did;
```

效果:人员表中的数据全部都显示,而 部门表中的数据符合条件的才会显示,不符合条件的会以 null 进行填充.也就是优先显示left join**左边表的数据，

**3.右外连接查询（右边表中的数据优先全部显示）**

```mysql
#查询人员和部门信息
select * from person right join dept on person.did= dept.did;
```

效果：与左外连接正好相反

**4 全连接查询(显示左右表中全部数据)**

　　全连接查询：是在内连接的基础上增加 左右两边没有显示的数据
　　注意: mysql并不支持全连接 full JOIN 关键字
　　注意: 但是mysql 提供了 UNION 关键字.使用 UNION 可以间接实现 full JOIN 功能

```mysql
#查询人员和部门的所有数据
 
SELECT * FROM person LEFT JOIN dept ON person.did = dept.did
UNION
SELECT * FROM person RIGHT JOIN dept ON person.did = dept.did;
```

#### 子查询

出现在其他语句中的select语句

可以出现的位置:

- select 后面
- from 后面
- where或having 后面
- exists 后面

#### 联合查询

合并多个结果集

union 

语法

```mysql
#语句1 union 语句2
#查询学生姓名，性别以及老师姓名，性别
SELECT teachername,teachersex from teacher_table UNION ALL
SELECT username,sex from student_table
```

注意：

- 多条查询语句的查询列数要求一致
- 查询语句的 每一列的类型和顺序最好一致
- union默认去重，使用union all可以包含所有

## 约束

在MySQL中，约束是指对表中数据的一种约束，能够帮助管理员更好的管理数据库，并且能够确保数据库中数据的正确性和有效性。

### 1）主键约束 primary key

主键约束是使用最频繁的约束，在设计数据表时，一般情况下，都会要求表中设置一个主键。

主键是表的一个特殊字段，该字段能唯一标识该表中的每条信息。例如：学生信息表中的学号是唯一的。

### 2）外键约束 foreign key

外检余数经常和主键约束一起使用，用来确保数据的一致性。

例如：一个水果摊，只有苹果、桃子。那么，你来到水果摊卖水果只能选择苹果和桃子，不能购买其他的水果

**用于限制两个表的关系，保证字段的值比逊来自于主表的关联列的值**

### 3）唯一约束 unique

唯一约束与主键约束有一个相似的地方，就是他们都能够确保列的唯一性。与主键约束不同的是，唯一约束在一个表中可以有多个，并且设置唯一约束的列是允许有空值得，虽然只能有一个。

例如：在用户信息表中，要避免表中的用户名重名，就可以把用户名列设置为唯一约束。

### 4）检查约束 check（mysql不支持）

检查约束是用来检查数据表中，字段值是否有效的一个字段。

例如：学生信息表中的年龄字段是没有负数的，并且数值也是有限制的，如果是大学生，年龄一般应该在18-30岁之间。在设置字段的检查约束是要根据实际情况进行设置，这样能减少无效的输入。

### 5）非空约束 not null

非空约束用来约束表中的字段不能为空，例如，在学生信息表中，如果不添加学生姓名，那么这条记录是没有用的

### 6）默认值约束 default

默认值约束用来约束数据表中某个字段不输入值时，自动为其添加一个已经设置好的值。

例如： 在注册学生信息时，如果不能输入学生的性别，那么会默认设置一个性别或者一个“未知”

**分类**

- 列级约束
  - 都支持，但外键约束没效果
- 表级约束
  - 除了非空、默认，其他都支持

#### 官方写法

在定义字段的同时指定主键，语法格式如下：

```<字段名> <数据类型> PRIMARY KEY [默认值]```

username varchar primary key default 1001 

其他约束类似

或者是在定义完所有字段之后指定主键，这是比较正规的写法，语法格式如下：

```mysql
[CONSTRAINT <约束名>] PRIMARY KEY [字段名]
[CONSTRAINT <外键名>] FOREIGN KEY 字段名 [，字段名2，…] REFERENCES <主表名> 主键列1 [，主键列2，…]
[CONSTRAINT <约束名>] UNIQUE [字段名]
```

username varchar,



age int ,

constraint pk primary key id,

constraint foreign key classid references class_table classid



##### 修改表时添加约束

```mysql
ALTER TABLE <数据表名> ADD PRIMARY KEY(<字段名>);
ALTER TABLE <数据表名> ADD CONSTRAINT <外键名>
FOREIGN KEY(<列名>) REFERENCES <主表名> (<列名>);
ALTER TABLE <数据表名> ADD CONSTRAINT <唯一约束名> UNIQUE(<列名>);
ALTER TABLE tb_emp7 ADD CONSTRAINT <检查约束名> CHECK(<检查约束>)；
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <数据类型> DEFAULT <默认值>;
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名>
<字段名> <数据类型> NOT NULL;
```



##### 删除约束

```mysql
ALTER TABLE <数据表名> DROP PRIMARY KEY;
ALTER TABLE <表名> DROP FOREIGN KEY <外键约束名>;
ALTER TABLE <表名> DROP INDEX <唯一约束名>;
ALTER TABLE <数据表名> DROP CONSTRAINT <检查约束名>;
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> DEFAULT NULL;
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> NULL;
```

查看表中约束：SHOW CREATE TABLE <数据表名>;


## 事务

transaction是一种机制，若干条数据操作指令构成一个操作序列，该序列要么全部执行，要么全部不执行，事务是一个不可分割的单元。

### 事务的四个特性，简称为acid

#### 原子性（atomicity）

事务是一个不可分割的最小工作单元，要么全部提交，要么全部失败回滚。
	**原理：undo log**

```
	innoDB存储引擎提供了undo log（回滚日志），在undo log中保存了和执行操作相反的记录,如果事务执行失败则会执行rollback，这时就需要使用到undo log中的日志记录。
```

#### 一致性（consistency）

在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。

#### 隔离性（isolation）

对数据进行修改的所有并发事务是彼此隔离的，这表明必须是独立的，它不应该以任何方式依赖或影响其他事务。 

数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。

简单说： 隔离性追求的是并发情形下事务之间互不干扰。

#### 持久性（durability）

事务处理结束后，对数据的修改就是永久的，即使系统故障也不会丢失。	

**原理： redo log**

```
	 数据存放在磁盘，每次都需要从磁盘读取数据，效率很低，所以innoDB提供了缓存，每次写入数据都是先写入缓存中，缓存中的数据会定时刷新磁盘。
		这样就带来了一个问题，数据写入缓存后，服务器宕机了怎么办，数据是不是就丢失了？要真数据丢失了，就没人用mysql了吧，
		每次写入时先写入redo log再写入缓存。就算宕机了，也可以从redo log中恢复数据，再更新缓存，保证了数据的持久性。
```



### 执行流程

```mysql
#开始事务
begin;  或 start transcation;
#提交事务
commit;
#回滚
rollback;
```

注意：

​	1.事务尽可能简短，事物的开启到结束会在数据库管理系统中保留大量资源，影响软件的性能

​	2.事务中访问的数据量尽量最少

​	3.查询数据时尽量不要使用事务，避免占用过多资源

​	4.在事务处理过程中尽量不要出现等待用户输入的操作

### 设置自动提交事务

通过命令查看事务当前状态

```mysql
SHOW VARIABLES LIKE 'autocommit';
```

设置自动提交

```mysql
set autocommit = 0|1|ON|OFF;
```

0或off为关闭自动提交，即遇到commit或者rollback才会结束一个事物

1或on表示开启自动提示，每一条sql语句即为一个事务

默认都是开启，自动提交事务

### jdbc

- 通过connection打开事务和提交

- 同一个connection

 ```java
public class TransactionTest {
  
    Connection connection=null;
    public static Connection getCon() throws Exception {
        Class.forName("com.mysql.jdbc.Driver");
        return DriverManager.getConnection("jdbc:mysql://localhost:3306/xsh?useUnicode=true&characterEncoding=UTF-8","root","Logan");
    }
  
    public static void update(int core,String name) throws Exception{
        String sql="update test_user set core=? where name=?";
        PreparedStatement statement = getCon().prepareStatement(sql);
        statement.setInt(1,core);
        statement.setString(2,name);
        statement.executeUpdate();
    }

  
    public static void main(String[] args) throws Exception{      
        Connection con=getCon();
        con.setAutoCommit(false);//开启事务
        try{
            update(20,"Logan");
            int a=5/0;
            con.commit();//提交
        }catch (Exception e){
            System.out.println("erro");
          	con.rollback();//回滚
        }
    }
  
 ```

  

### 隔离级别

如果没有隔离级别，当事务访问数据库中相同数据时，会导致一些并发问题。

- 脏读 dirty read ： 读取到另一个事务未提交的更新数据，也就是脏数据

- 不可重复读 unrepeatable read：在同一事务中，对同一记录的两次读取不一致，因为另一事务对该记录做了修改

- 幻读（虚读）phantom read：对同一张表的两次查询不一致，因为另一事务插入了一条记录。

  不可重复读和幻读的区别:

  ​	不可重复读是读取到了另一事务的更新

  ​	幻读是读到了另一事务的插入

一个事务和其他事物的隔离程度成为隔离级别，级别越高，数据一致性越好，但并发性越弱

#### 四大隔离级别

- serializable 串行化

  不会出现任何并发问题，对同一数据访问是串行的，并发访问性能最差

- Repeatable read 可重复度 （mysql默认）

  防止脏读和不可重复读

  性能比上一个好

- read committed 读已提交数据（Oracle 默认）

  防止脏读

  性能比上一个好

- read uncommitted 读未提交数据
	可能出现任何事物并发问题
	性能最好

#### 四种隔离级别可能出现的并发问题

| 隔离级别                         | 脏读（Dirty Read） | 不可重复读（NonRepeatable Read） | 幻读（Phantom Read） |
| -------------------------------- | ------------------ | -------------------------------- | -------------------- |
| **读未提交（Read uncommitted）** | 可能               | 可能                             | 可能                 |
| **读已提交（Read committed）**   | 不可能             | 可能                             | 可能                 |
| **可重复读（Repeatable read）**  | 不可能             | 不可能                           | 可能                 |
| **可串行化（Serializable ）**    | 不可能             | 不可能                           | 不可能               |
**查看隔离级别**

```mysql
select @@tx_isolation
或
show variables like '%tx_isolation%'
```

**设置隔离级别**		

```mysql
	set [session | clobal] transaction isolation level [read uncommitted | read committed | read repeatable]
```

