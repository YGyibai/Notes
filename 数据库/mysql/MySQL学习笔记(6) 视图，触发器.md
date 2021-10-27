# MySQL学习笔记(6) 视图，触发器

### 1. 背景

本文讲视图和触发器。

### 2.视图 ( View )

>  视图是一个虚拟表，它也有行和列。它通过引用的方式指向数据。 

#### **创建视图**

```mysql
CREATE VIEW view_1 AS
  SELECT * FROM tb_table1 WHERE class = 1;
```

#### **修改视图** 

使用   CREATE OR REPLACE VIEW 来修改。

```mysql
  CREATE OR REPLACE VIEW view_1 AS
    SELECT * FROM tb_table1 WHERE class = 1;
```

或者使用 ALTER VIEW 来修改视图

```mysql
ALTER VIEW view_1 AS 
    SELECT * FROM tb_table1 WHERE class = 1;
```

#### **删除视图**

```mysql
DROP VIEW IF EXISTS view_1;
```

### 3. 触发器 ( Trigger )

>  触发器是一段嵌入到[MySQL](https://cloud.tencent.com/product/cdb?from=10680)的SQL脚本程序，有点类似存储过程。它由事件来触发，这些事件有 INSERT， UPDATE， DELETE 等。定义了触发器以后，当执行这些语句的时将触发事件，对应事件的触发器脚本将被激活和执行。 

#### 3.1 创建触发器

```mysql
DELIMITER //
CREATE TRIGGER trggger1 
    BEFORE INSERT ON account
    FOR EACH ROW
  BEGIN
    SET @sum = @sum + NEW.money;
  END //
DELIMITER ;
```

#### 3.2 删除触发器

```mysql
DROP TRIGGER trggger1;
```

END

