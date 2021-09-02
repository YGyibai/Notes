# MySQL学习笔记(4) - 数据类型和运算符

### 1. 背景

本文讲[MySQL](https://cloud.tencent.com/product/cdb?from=10680)的数据类型和运算符。

### 2.数据类型

MySQL 支持多种数据类型，主要有：

-  **数值类型：**包括 整数型 TINYINT, SMALLINT, MEDIUMINT, INT, BIGINT, 浮点数 FLOAT, DOUBLE, 定点小数类型 DECIMAL。
-  **日期时间类型：**包括 YEAR，TIME， DATE， DATETIME ，和 TIMESTAMP
-  **字符串类型：**包括 CHAR, VARCHAR,  BINARY, VARBINRAY, BLOB, TEXT, ENUM 和 SET 等。字符串类型有分为 文本字符串和二进制字符串。



### 3. 运算符

-  **算术运算符：**用于各类数值运算。包括 加（+），减去（-），乘（*），除（/)，求余取模（%）
-  **比较运算符：**用于比较运算，包括  >, < ,等于 = ，以及 IN，BETWEEN AND， IS NULL， GRETEST， LEAST， LIKE，REGEXP。
- **逻辑运算符有：** 逻辑与，逻辑或，逻辑非等，逻辑异或。
- **位操作运算符：**包括 位与，位或，位非，位异或，左移，右移等。

##4. 函数

-  **数据函数**：绝对值函数 ABS，圆周率函数PI ；平方根 SQRT, 求余函数 MOD；取整函数CEIL, CEILING,和  FLOOR; 随机数 RAND;近似值 ROUND, TRUNCATE；符号函数 SIGN; 幂运算函数 POW，POWER，和EXP； 对数函数LOG；角度与弧度转换的函数 RADIANS和 DEGREES；正弦函数 SIN 和反正弦函数 ASIN；余弦函数 COS和反余弦函数 ACOS； 正切函数，反正切函数和余切函数；
-  **字符串函数**：字符串长度函数CHAR_LENGTH, LENGTH；合并函数CONCAT，CONCAT_WS；替换字符串的函数 INSERT；字母大小写转换函数 LOWER，LCASE，UPPER，UCASE；获取指定长度的字符串函数 LEFT，RIGHT；填充字符串的函数 LPAD，RPAD；删除空格的函数 LTRIM，RTRIM，TRIM；重复生成字符串的函数 REPEAT；空格函数 SPACE和替换函数REPLACE；比较字符串带下的函数 STRCMP；获取子串的函数 SUBSTRING和MID；匹配子串开始位置的函数 LOCATE；翻转函数 REVERSE；返回指定位置的字符串函数 ELT；返回字符串位置的函数 FIELD；返回子串位置的函数 FIND_IN_SET；选取字符串的函数 MAKE_SET；
- **日期和时间函数**：CURDTE; CURRENT_TIME; CURRENT_TIMESTAMP, LOCALTIME, NOW, SYSDATE()；UNIX时间戳函数。UTC_DATE；MONTH， MONTHNAME；WEEK；
-  **条件判断函数**：IF 函数；IFNULL函数；CASE函数；
- **系统信息函数:** VERSION(); USER(); 获得最后一个自动生成的ID值的函数 LAST_INERT_ID(); 加密函数 PASSWORD;MD5;ENCODE,DECODE;

