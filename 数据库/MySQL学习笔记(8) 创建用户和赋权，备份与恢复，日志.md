# MySQL学习笔记(8) 创建用户和赋权，备份与恢复，日志

### 1. 背景

一般在开发中，我们需要新建一个账户，并赋予某个[数据库](https://cloud.tencent.com/solution/database?from=10680)的访问权限。本文说下操作方法。

### 2.创建用户和赋权

**创建用户**

```mysql
CREATE USER 'zyf'@'%' identified by 'zyf';
```

- CREATE USER 关键字用于建立一个用户
- @ 符号前面是用户名，后面是主机名。一般是 locaohost 指本机。
- 注意： % 指代的主机名意思是“任何位置都可以登录使用"，也就是开启了远程登录。

**赋予操作权限**

```mysql
GRANT SELECT,INSERT,UPDATE,DELETE ON zoo.* to 'zyf'@'%';
```

- GRANT  关键字用于赋予权限
- 后面跟的 SELECT,INSERT,UPDATE,DELETE 是指增删改查方法都可以。
- ON 后跟 用户名和主机地址

### 3. 备份和恢复

**备份数据库**

```mysql
mysqldump -u root -p zoo > backup2021-06-24.sql
```

- mysqldump 关键字用于备份数据库
- 其后跟了用户名，和数据库名
- ">" 大于号后 跟上 一个文件名

**恢复数据库**

```mysql
# 先创建数据库
create database zoo2;
# 使用关键字  mysql
mysql zoo2 < backup2021-06-24.sql -u root -p
```

- 恢复数据时，使用 [mysql](https://cloud.tencent.com/product/cdb?from=10680) 关键字，而不是 mysqldump，这点要注意

### 4. MySQL 日志

MySQL 有四类日志：

- 错误日志：记录了MySQL服务出现的问题
- 查询日志：记录了客户端连接和执行的SQL语句
- 慢查询日志: 记录了执行时间过长的查询
- 二进制日志：记录了所有更改数据的语句

#### 5. 扩展

**查看MySQL数据库文件的位置**

```mysql
mysql> show global variables like "%datadir%";
```

END