# MySQL学习笔记(5) 增删改查，高级查询，和索引

### 1. 背景

本文讲查询数据。

### 2.知识

>  基本的就是 增删改查。一般说 CRUD, CRUD是指在做计算处理时的增加(Create)、检索(Retrieve)、更新(Update)和删除(Delete)几个单词的首字母简写。 

#### 2.1 简单查询

SELECT 语句用于从[数据库](https://cloud.tencent.com/solution/database?from=10680)中检索查询。 示例：

```mysql
select * from tb_table1 where name='li4'```
```

- where 关键字后面跟查询条件
- from 关键字后面跟表名或者视图名
- order by 后跟排序的字段

#### 2.2 插入

使用 insert into 语句可以插入一条数据记录。

```mysql
insert into tb_table1 (name,deptId) values ('wang',0);
```

- insert into 后跟表名，括号内写字段名
- values 后的括号内写具体字段对应的值

#### 2.2 更新

update 用于更新一条数据记录的值。

```mysql
update tb_table1 set name='zhang33' where id=1;
```

- update 后跟 表名
- set 后跟 修改的字段和值
- where 指定筛选条件

#### 2.3 删除

delete  用于删除一条数据记录。

```javascript
delete from tb_table1 where name='li4';
```

- delete 后跟 表名
- where 指定筛选条件

#### 2.4 高级查询

**是否包含在内 --- 使用 IN 关键字的查询**

```mysql
select * from tb_table1 where id in (1,3,4);
select * from tb_table1 where name in ('wang','zhang33');
```

**范围区间查询 --- 使用 BETWEEN AND 关键字的查询**

```mysql
select * from tb_table1 where id between 2 and 4;
```

**字符串模糊搜索 --- 使用  LIKE 关键字的查询**

```mysql
select * from tb_table1 where name like 'zh%'
```

- % 百分号是通配符，这里表示 zh 开头的都查询出来。

**查看空值（NULL） --- 使用  IS NULL 关键字的查询**

```mysql
select * from tb_table1 where salary is NULL;  # 正确
select * from tb_table1 where salary = NULL; # 错误的，查不到结果。
```

**多条件查询 --- 使用 AND 、OR关键字的查询**

```mysql
select * from tb_table1 where deptId=0 and salary is null;
```

**多字段排序 --  Order by 后使用多个字段**

```mysql
select * from tb_table1 order by name, deptId;
```

**分组 -- 使用 group by**

```mysql
select count(*) from tb_table1 group by class;
select count(*),class from tb_table1 group by class;
```

**分组后再过滤 -- 在 group by 中使用 having**

```mysql
select count(*),class from tb_table1 group by class having class = 1;
```

**分页查询 -- 使用 LIMIT**

```mysql
mysql> select * from tb_table1 limit 4,2;
```

- LIMIT 后第一个数字 指 跳过多少行。
- LIMIT 后逗号后的数字指 取多少行。

**计数，求和，平均，取最大最小值 -- 使用聚合函数**

```mysql
select count(deptId),class from tb_table1 group by class;
select sum(deptId),class from tb_table1 group by class;
select avg(deptId),class from tb_table1 group by class;
select min(Id),class from tb_table1 group by class;
select max(Id),class from tb_table1 group by class;
```

**连接查询： 内连接，左连接，右连接**

```mysql
# 内连接 inner join  
select * from tb_table1 as t inner join account as a on t.id = a.userId;
# 左连接
select * from tb_table1 as t left join account as a on t.id = a.userId;
# 右连接
select * from tb_table1 as t right join account as a on t.id = a.userId;
```

**子查询， ANY SOME IN 等**

```mysql
select * from tb_table1 where id IN (select userId from account WHERE money>100)
```

**合并查询 -- 使用 UNION**

```mysql
select * from tb_table1 where deptId =1 union select * from tb_table1 where deptId =2;
```

**正则表达式查询 -- 使用 REGEXP**

```mysql
select * from tb_table1 WHERE name REGEXP '^z';
```

参考：[https://dev.mysql.com/doc/refman/8.0/en/explain.html](https://links.jianshu.com/go?to=https%3A%2F%2Fdev.mysql.com%2Fdoc%2Frefman%2F8.0%2Fen%2Fexplain.html)

END

