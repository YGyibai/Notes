<font color="blue">**SQL基础和各数据库语法差异**</font>

### 检索数据

```sql
select 列名 from 表名`
```

#### 限定检索

<font color="#ff00ff">返回一定数量的行数据</font>

**SQL Server 和 Access** 中使用 SELECT 时，可以使用 **TOP** 关键字来限制

```sql
SELECT TOP 5 prod_name
FROM Products;
```

 **DB2**，很可能习惯使用下面这一 DBMS 特定的 SQL 语

```sql
SELECT prod_name
FROM Products
FETCH FIRST 5 ROWS ONLY;
```

**Oracle**，需要基于 **ROWNUM**(行计数器)来计算行

```sql
SELECT prod_name
FROM Products
WHERE ROWNUM <=5;
```

**MySQL、MariaDB、PostgreSQL 或者 SQLite**，需要使用 **LIMIT** 子句

```sql
SELECT prod_name
FROM Products
LIMIT 5; -- 一个参数代表数量 limit 2,3 第一参数：检索下标，第二参数：检索数量
```

**DISTINCT关键字**

**DISTINCT**关键字，顾名思义，它指示数据库只返回不同的值。
如果使用**DISTINCT**关键字，它**必须直接放在列名的前面。**

```sql
SELECT DISTINCT vend_id
FROM Products;  -- 根据列名去重 
```

>DISTINCT 关键字作用于所有的列，不仅仅是跟在其后的那一列。
>例如，你指定 SELECT DISTINCT vend_id, prod_price，除非指定的两列完全相同，否则所有的行都会被检索出来。 

#### 排列检索数据

**关键字 ORDER BY** 默认升序

```sql
    -- 按单列排序
    SELECT prod_name
    FROM Products
    ORDER BY prod_name;
    -- 按多列排序，先按价格再按名字排序
    SELECT prod_id, prod_price, prod_name
    FROM Products
    ORDER BY prod_price, prod_name;

    -- 按列位置排序 先按第2列列名排序，再按第3列列名排序
    SELECT prod_id, prod_price, prod_name
    FROM Products
    ORDER BY 2, 3;
```

>  ORDER BY 子句对检索出的数据进行排序。这个子句必须是 SELECT 语句中的最后一条子句

**关键字 DESC** 降序排列

```sql
    -- DESC 关键字只应用到直接位于其前面的列名。
    SELECT prod_id, prod_price, prod_name
    FROM Products
    ORDER BY prod_price DESC, prod_name;
```
> 警告:在多个列上降序排序
> 必须对每一列指定 DESC 关键字。

### 过滤数据

只检索所需数据需要指 定搜索条件(search criteria)，搜索条件也称为过滤条件(filter condition)。

**WHERE 关键字**

```sql
    SELECT prod_name, prod_price
    FROM Products
    WHERE prod_price = 3.49;
```
**BETWEEN AND 关键字 范围值检查**

```sql
    SELECT prod_name, prod_price
    FROM Products
    WHERE prod_price BETWEEN 5 AND 10;
```

**IS NULL 空值检查**

```sql
    SELECT prod_name
    FROM Products
    WHERE prod_price IS NULL;
```

**AND、 OR 关键字**

> AND
> 用在 WHERE 子句中的关键字，用来指示检索满足所有给定条件的行。

> OR
> WHERE 子句中使用的关键字，用来表示检索匹配任一给定条件的行。

> IN
> WHERE 子句中用来指定要匹配值的清单的关键字，功能与 OR 相当

> NOT
> WHERE 子句中用来否定其后条件的关键字。

```
    在 WHERE 子句中使用圆括号任何时候使用具有 AND 和 OR 操作符的 WHERE 子句，
都应该使用圆括 号明确地分组操作符。不要过分依赖默认求值顺序，
即使它确实如你 希望的那样。使用圆括号没有什么坏处，它能消除歧义。
```


#### 拼接字段

**拼接(concatenate)**
将值联结到一起(将一个值附加到另一个值)构成单个值。

>    在 SQL 中的 SELECT 语句中，可使用一个特殊的操作符来拼接两个列。根据你所使用的 DBMS，此操作符可用 加号(+)或两个竖杠(||)表示。在 MySQL 和 MariaDB 中，必须使用 特殊的函数。

>**说明:是+还是||?**
>Access 和 SQL Server 使用+号。
>DB2、Oracle、PostgreSQL、SQLite 和 Open Office Base 使用||。
>详细请参阅具体的 DBMS 文档。
字符串拼接
```sql
    -- Access 和 SQL Server
    SELECT vend_name + ' (' + vend_country + ')'
    FROM Vendors
    ORDER BY vend_name;
    --  MySQL 或 MariaDB 时需要使用的语句:
    SELECT Concat(vend_name, ' (', vend_country, ')')
    FROM Vendors
    ORDER BY vend_name;
    -- DB2、Oracle、PostgreSQL、SQLite 和 Open Office Base 使用||
    SELECT vend_name || ' (' || vend_country || ')'
    FROM Vendors
    ORDER BY vend_name;
```
```sql
    #结果
    vend_title ----------------------------------------------------------- Bear Emporium (USA)
    Bears R Us (USA)
    Doll House Inc. (USA)
    Fun and Games (England)
    Furball Inc. (USA)
    Jouets et ours (France)
```

 **去空格 TRIM**

    >TRIM 函数
    >大多数 DBMS 都支持 RTRIM()(正如刚才所见，它去掉字符串右边的 空格)、LTRIM()(去掉字符串左边的空格)以及 TRIM()(去掉字符 串左右两边的空格)。

### 函数

#### 用于处理文本字符串

|  TRIM函数    |   函数内容   |
| ---- | ---- |
| RTRIM()   | 去掉字符串右边空格 |
|  LTRIM() | 去掉字符串左边空格 |
| TRIM() | 去掉字符串左右边空格 |
| UPPER() | 字符串大写 |
| LENGTH() | 返回字符串长度 |
| LEFT() | 返回字符串左边的字符 |
| RIGHT() | 返回字符串右边的字符 |

#### 用于在数值数据上进行算术操作

ABS() 返回一个数的绝对值

COS() 返回一个角度的余弦 

EXP() 返回一个数的指数值

PI() 返回圆周率

SIN() 返回一个角度的正弦

SQRT() 返回一个数的平方根

TAN() 返回一个角度的正切

**用于处理日期和时间值并从这些值中提取特定成分**

**返回 DBMS 正使用的特殊信息(如返回用户登录信息)的系统函数**

#### 聚合函数

AVG()           返回某列的平均值
COUNT()         返回某列的行数      两种用法 count(*)返回表中行的总数   count(列名)返回当前列条数(会忽略nul值)
MAX()           返回某列的最大值    (会忽略null值)
MIN()           返回某列的最小值    (会忽略null值)
SUM()           返回某列之和        也可以合计计算值 SUM(列名*列名)

### 分组数据

**GROUP BY关键字**
返回每个供应商  提供的产品数目
输入▼
```sql
    SELECT vend_id, COUNT(*) AS num_prods
    FROM products
    GROUP BY vend_id;
```
输出▼
vend_id     num_prods
-------     ---------
BRS01       3
DLL01       4
FNG01 2

使用规定▼
- GROUP BY 子句可以包含任意数目的列，因而可以对分组进行嵌套， 更细致地进行数据分组
- 如果在SQL语句中有（SELECT 之后）聚合函数，数据将在最后指定的分组上进行汇总
- GROUP BY 子句中列出的每一列都必须是检索列或有效的表达式
- 如果分组列中有null值，则null将作为一个分组返回
- GROUP BY 子句必须出现在 WHERE 子句之后，ORDER BY 子句之前

**HAVING关键字**
和WHERE关键字的区别：
- HAVING 支持所有 WHERE 操作符
- WHERE 在数据分组前进行过滤，HAVING 在数据分组后进行过滤
列出具有两个以上产品且其价格 大于等于 4 的供应商:
输入▼
```sql
    SELECT vend_id, COUNT(*) AS num_prods
    FROM Products
    WHERE prod_price >= 4
    GROUP BY vend_id
    HAVING COUNT(*) >= 2;
```
输出▼
vend_id     num_prods
-------     -----------
BRS01       3
FNG01       2

### 联结表

#### 内联结(等值联结)
输入▼
```sql
    SELECT vend_name, prod_name, prod_price
    FROM vendsor INNER JOIN  products
    ON vendsor.vend_id = products.vend_id;
```
联结多个表
    显示订单 20007 中的物品。订单物品存储在 OrderItems 表中。
    输入▼
```sql
    SELECT prod_name, vend_name, prod_price, quantity 
    FROM OrderItems, Products, Vendors
    WHERE Products.vend_id = Vendors.vend_id
    AND OrderItems.prod_id = Products.prod_id
    AND order_num = 20007;
```
> 注意：简单的等值语 WHERE 和 INNER JOIN 都可以，具体看那个更顺手

#### 自联结

> 查询中需要的两个表实际上是相同的表
与 Jim Jones 同一公司的所有顾客 (两种方式)
- 子查询语句
输入▼
```sql
    SELECT cust_id, cust_name, cust_contact
    FROM Customers
    WHERE cust_name = (
        SELECT cust_name
        FROM Customers
        WHERE cust_contact = 'Jim Jones'
    );
```
- 自联结语句
```sql
    SELECT c1.cust_id, c1.cust_name, c1.cust_contact
    FROM Customers AS c1,Customers AS c2
    WHERE c1.cust_name=c2.cust.cust_name
    AND c2.cust_contact= 'Jim Jones';
```
#### 外联结
检索包括没有订单顾客在内的所有顾客

输入▼
左外联结
```sql
    SELECT Customers.cust_id, Orders.order_num
    FROM Customers LEFT OUTER JOIN Orders
    ON Customers.cust_id = Orders.cust_id;
```
右外联结
```sql
    SELECT Customers.cust_id, Orders.order_num
    FROM Customers RIGHT OUTER JOIN Orders
    ON Orders.cust_id = Customers.cust_id;
```
>在使用 OUTER JOIN 语法时，必须使用 RIGHT 或 LEFT 关键字指定包括其所有行的表
>(RIGHT 指出的是 OUTER JOIN 右边的表，而 LEFT 指出的是 OUTER JOIN 左边的表)。
>上面的例子使用 LEFT OUTER JOIN >从 FROM 子句左边的表 (Customers 表)中选择所有行。
>为了从右边的表中选择所有行，需要使用RIGHT OUTER JOIN，

全外联结
输入▼
```sql
    SELECT Customers.cust_id, Orders.order_num
    FROM Orders FULL OUTER JOIN Customers
    ON Orders.cust_id = Customers.cust_id;
```
>全外联结包含两个表的不关联的行
>注意：FULL OUTER JOIN 
>Access、MariaDB、MySQL、Open Office Base 和 SQLite 不支持 FULL OUTER JOIN 语法


