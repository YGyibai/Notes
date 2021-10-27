# MySQL学习笔记(7) 存储过程和函数

### 1. 背景

本节学习存储过程和函数。

### 2.知识

#### 2.1 概念

>  存储过程是多条SQL语句的集合，即一次执行多个语句，批量处理SQL语句。 

存储过程和函数很类似，概念不同，使用的方法不同。但实际使用上都是用来一次执行一批（多个）SQL。

#### 2.2 存储过程

**创建存储过程** 先用 DELIMITER 将 [MySQL](https://cloud.tencent.com/product/cdb?from=10680) 结束符设置 //，因为MySQL 默认的结束符是 分号( ; ) ，这样是为了避免冲突。写完存储过程后，再改回 分号。 示例：

```mysql
DELIMITER //
CREATE PROCEDURE ppp()
BEGIN
  SELECT * FROM book;
END //
DELIMITER ;
```

**调用存储过程**

```mysql
CALL ppp(); 
```

**删除存储过程**

```mysql
DROP PROCEDURE ppp;
```

#### 2.3 函数

**创建函数**

```mysql
DELIMITER //
CREATE FUNCTION fun1() RETURNS CHAR(50)
  RETURN (SELECT bookName from book);
//
DELIMITER ;
```

执行前注意： MySQL 默认不允许（信任）创建函数，可通过下面的预计关闭限制：

```javascript
SET GLOBAL log_bin_trust_function_creators = 1;
```

**调用函数** 和普通函数调用一样

```mysql
SELECT fun1();
```

**删除函数**

```mysql
DROP FUNCTION fun1;
```

# 3. 扩展

整体看存储过程的维护成本还是很高的，一般的公司没有DBA的话确存在困难，建议把业务逻辑放在业务层做。网上也在使用存储过程上也存在一些讨论，可以了解下：

为什么阿里巴巴Java开发手册里要求禁止使用存储过程？ [https://www.zhihu.com/question/57545650?sort=created](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.zhihu.com%2Fquestion%2F57545650%3Fsort%3Dcreated)

END

