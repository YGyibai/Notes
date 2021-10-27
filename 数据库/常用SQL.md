### 多行合并成一行

**1.使用条件查询 查询部门为20的员工列表**

DEPTNO ：部门编号  ENAME：员工姓名

```sql
-- 查询部门为20的员工列表
SELECT t.DEPTNO,t.ENAME FROM SCOTT.EMP t where t.DEPTNO = '20' ;
```

**2.Oracle中可以使用  listagg() WITHIN GROUP ()  将多行合并成一行(\**比较常用\**)**

```sql
SELECT
	T .DEPTNO,
	listagg (T .ENAME, ',') names
FROM
	SCOTT.EMP T
WHERE
	T .DEPTNO = '20' 
```

**3.MySQL使用  group_concat()  将多行合并成一行(比较常用)**

```sql
SELECT 
  T.DEPTNO,
	group_concat ( T.ENAME ORDER BY T.ENAME separator ',' ) 
FROM
	EMP T 
WHERE
	T.DEPTNO = '20' 
GROUP BY
	T.DEPTNO;
```

