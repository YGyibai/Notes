## 概念（Mybatis是什么）
1.	一个持久层框架,它支持自定义SQL、存储过程以及高级映射
2.	Mybatis消除了几乎所有的**JDBC代码**以及**设置参数**和**获取结果集**的工作
3.	Mybatis可以通过简单的**XML**或**注解**来配置和映射：原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式Java对象）为数据库中的记录。

## Mybatis的由来
- MyBatis 本是apache的一个开源项目iBatis。
- 2010年这个项目由apache software foundation 迁移到了google  code并且改名为MyBatis。
- 2013年11月迁移到Github。

## Mybatis编码流程
1.	建立maven项目
2.	编写pom文件
3.	建立mybatis-config.xml
4.	POJO类（实体类和表数据一一对应）
5.	映射文件：编写xxxMappe.xml
6.	编写dao代码： xxxDao接口、xxxxDaoimpl实现类
7.	单元测试类 SqlSessionFactory的创建和获取SqlSession

## 项目搭建
pom文件依赖
```xml
<dependencies>
        <!-- mybatis依赖 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.6</version>
        </dependency>
        <!-- mysql依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.21</version>
        </dependency>
        <!-- junit依赖 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.8.1</version>
        </dependency>
    </dependencies>    
```
mybatis-config.xml 文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration><!-- 配置 -->
    <properties><!-- 属性 -->
        <property name="driver" value="com.mysql.cj.jdbc.Driver" />
        <property name="url" value="jdbc:mysql://localhost:3306/xsh?characterEncoding=utf8" />
        <property name="username" value="root" />
        <property name="password" value="Logan" />
    </properties>
    <!--数据库环境-->
    <environments default="development"><!-- 配置环境 -->
        <environment id="development"><!-- 环境变量 -->
            <transactionManager type="JDBC" /><!-- 事务管理器 -->
            <dataSource type="POOLED"><!-- 数据源 -->
                <property name="driver" value="${driver}" />
                <property name="url"    value="${url}" />
                <property name="username" value="${username}" />
                <property name="password" value="${password}" />
            </dataSource>
        </environment>
    </environments>
    <!-- 映射文件 -->
    <mappers><!-- 映射器 -->
        <mapper resource="TestClassMapper.xml" />
    </mappers>
</configuration>
```
TestClassMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mapper">
    <select id="findById" parameterType="int" resultType="pojo.TestClass">
        select * from test_class where id=#{id}
    </select>

</mapper>
```

测试类
```java
public class Test {
    private SqlSessionFactory factory;
    @Before
    public void getFactory(){
        try {
            String url="mybatis-config.xml";
            InputStream input= Resources.getResourceAsStream(url);
            factory = new SqlSessionFactoryBuilder().build(input);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    @org.junit.Test
    public void findById1(){
        SqlSession session=factory.openSession();
        TestClassMapper mapper=session.getMapper(TestClassMapper.class);
        System.out.println(mapper.findById(1));
        session.commit();
        session.close();
    }

}
```
<font color="red">**保持四大原则：命名空间和对应的接口名以及全路径一致，参数类型一致，方法名一致，返回值类型一致**</font>



## mybatis核心组件 


1）SqlSessionFactoryBuilder（构造器）：最佳作用域是方法作用域（也就是局部方法变量）
	它会根据配置或者代码来生成 SqlSessionFactory，采用的是分步构建的 Builder 模式。

2）SqlSessionFactory（工厂接口）：最佳作用域是应用作用域（全局范围）
	依靠它来生成 SqlSession，使用的是工厂模式。


3）SqlSession（会话）：它的最佳的作用域是请求或方法作用域
	一个既可以发送 SQL 执行返回结果，也可以获取 Mapper 的接口。在现有的技术中，一般我们会让其在业务逻辑代码中“消失”，而使用的是 MyBatis 提供的 SQL Mapper 接口编程技术，它能提高代码的可读性和可维护性。

4）SQL Mapper（映射器）:最佳作用域是方法作用域内
	MyBatis 新设计存在的组件，它由一个 Java 接口和 XML 文件（或注解）构成，需要给出对应的 SQL 和映射规则。它负责发送 SQL 去执行，并返回结果。

## Mybatis核心原理
**JDBC有四个核心对象：**
（1）DriverManager，用于注册数据库连接
（2）Connection，与数据库连接对象
（3）Statement/PrepareStatement，操作数据库SQL语句的对象
（4）ResultSet，结果集或一张虚拟表

**而MyBatis也有四大核心对象**：
（1）SqlSession对象，该对象中包含了执行SQL语句的所有方法。类似于JDBC里面的Connection。
（2）Executor接口，它将根据SqlSession传递的参数动态地生成需要执行的SQL语句，同时负责查询缓存的维护。类似于JDBC里面的Statement/PrepareStatement。
（3）MappedStatement对象，该对象是对映射SQL的封装，用于存储要映射的SQL语句的id、参数等信息。
（4）ResultHandler对象，用于对返回的结果进行处理，最终得到自己想要的数据格式或类型。可以自定义返回类型。

<img src="http://c.biancheng.net/uploads/allimg/190704/5-1ZF4130T31N.png" alt="MyBatis框架的执行流程图" style="zoom:%;" />

**1）读取 MyBatis 配置文件：**
	mybatis-config.xml 为 MyBatis 的全局配置文件，配置了 MyBatis 的运行环境等信息，例如数据库连接信息。

**2）加载映射文件。**
	映射文件即 SQL 映射文件，该文件中配置了操作数据库的 SQL 语句，需要在 MyBatis 配置文件 mybatis-config.xml 中加载。mybatis-config.xml 文件可以加载多个映射文件，每个文件对应数据库中的一张表。

**3）构造会话工厂：**
	通过 MyBatis 的环境等配置信息构建会话工厂 SqlSessionFactory。

**4）创建会话对象：**
	由会话工厂创建 SqlSession 对象，该对象中包含了执行 SQL 语句的所有方法。

**5）Executor 执行器：**
	MyBatis 底层定义了一个 Executor 接口来操作数据库，它将根据 SqlSession 传递的参数动态地生成需要执行的 SQL 语句，同时负责查询缓存的维护。

**6）MappedStatement 对象：**
	在 Executor 接口的执行方法中有一个 MappedStatement 类型的参数，该参数是对映射信息的封装，用于存储要映射的 SQL 语句的 id、参数等信息。

**7）输入参数映射：**
	输入参数类型可以是 Map、List 等集合类型，也可以是基本数据类型和 POJO 类型。输入参数映射过程类似于 JDBC 对 preparedStatement 对象设置参数的过程。

**8）输出结果映射：**
	输出结果类型可以是 Map、 List 等集合类型，也可以是基本数据类型和 POJO 类型。输出结果映射过程类似于 JDBC 对结果集的解析过程。

## 增删改查属性

### select方法

| 属性名称       | 描 述                                                        |
| -------------- | ------------------------------------------------------------ |
| id             | 它和 Mapper 的命名空间组合起来使用，是唯一标识符，供 MyBatis 调用 |
| parameterType  | 表示传入 SQL 语句的参数类型的全限定名或别名。它是一个可选属性，MyBatis 能推断出具体传入语句的参数 |
| resultType     | SQL 语句执行后返回的类型（全限定名或者别名）。如果是集合类型，返回的是集合元素的类型，返回时可以使用 resultType 或 resultMap 之一 |
| resultMap      | 它是映射集的引用，与 <resultMap> 元素一起使用，返回时可以使用 resultType 或 resultMap 之一 |
| ==flushCache== | 用于设置在调用 SQL 语句后是否要求 MyBatis 清空之前查询的本地缓存和二级缓存，默认值为 false，如果设         置为 true，则任何时候只要 SQL 语句被调用都将清空本地缓存和二级缓存 |
| ==useCache==   | 启动二级缓存的开关，默认值为 true，表示将査询结果存入二级缓存中 |
| timeout        | 用于设置超时参数，单位是秒（s），超时将抛出异常              |
| fetchSize      | 获取记录的总条数设定                                         |
| statementType  | 告诉 MyBatis 使用哪个 JDBC 的 Statement 工作，取值为 STATEMENT（Statement）、 PREPARED（PreparedStatement）、CALLABLE（CallableStatement） |
| resultSetType  | 这是针对 JDBC 的 ResultSet 接口而言，其值可设置为 FORWARD_ONLY（只允许向前访问）、SCROLL_SENSITIVE（双向滚动，但不及时更新）、SCROLLJNSENSITIVE（双向滚动，及时更新） |

### insert

执行完一条插入语句后将返回一个整数表示其影响的行数，它的属性与 <select> 元素的属性大部分相同，以下为特有属性：

- keyProperty：该属性的作用是将插入或更新操作时的返回值赋给 PO 类的某个属性，通常会设置为主键对应的属性。如果是联合主键，可以将多个值用逗号隔开。

- keyColumn：该属性用于设置第几列是主键，当主键列不是表中的第 1 列时需要设置。如果是联合主键，可以将多个值用逗号隔开。
- useGeneratedKeys：该属性将使 MyBatis 使用 JDBC 的 getGeneratedKeys（）方法获取由数据库内部产生的主键，例如 [MySQL](http://c.biancheng.net/mysql/)、SQL Server 等自动递增的字段，其默认值为 false。

#### 获得刚插入数据主键的两种方法

1. 如果数据库支持主键自增，可以用如下方法

   ```xml
   <!--添加一个用户，成功后将主键值返回填给uid(po的属性)-->
   <insert id="addUser" parameterType="com.po.MyUser" keyProperty="uid" useGeneratedKeys="true">
       insert into user (uname,usex) values(#{uname},#{usex})
   </insert>
   ```

   keyProperty设置获得的主键和实体类中哪个变量关联

   useGeneratedKeys设置开启获取主键

2. 如果数据库不支持主键自动递增，例如oracle，或者没有使用主键自增，可以使用<selectKey>标签

   ```xml
   <insert id="insertUser" parameterType="com.po.MyUser">
       <!-- 先使用selectKey元素定义主键，然后再定义SQL语句 -->
       <selectKey keyProperty="uid" resultType="Integer" order="BEFORE">
       select if(max(uid) is null,1,max(uid)+1) as newUid from user)
       </selectKey>
       insert into user (uid,uname,usex) values(#{uid},#{uname},#{usex})
   </insert>
   
   #或
   <insert id="insert" parameterType="com.pojo.TestTable" >
   	<selectKey resultType="int" keyProperty="id" order="AFTER" >
       	select LAST_INSERT_ID()
        </selectKey>
           insert into test_table values(null,#{username},#{money})
   </insert>
   
   ```

   第一种来自C语言中文网，是在插入之前获取，第二种是插入后获取

### update和delete

update> 和 <delete> 元素比较简单，它们的属性和 <insert> 元素、<select> 元素的属性差不多，执行后也返回一个整数，表示影响了数据库的记录行数

### sql

<sql> 元素的作用在于可以定义 SQL 语句的一部分（代码片段），以方便后面的 SQL 语句引用它，例如反复使用的列名。

```xml
<sql id="comColumns">id,uname,usex</sql>
<select id="selectUser" resultType="com.po.MyUser">
    select <include refid="comColumns"> from user
</select>
```

## Mybatis标签

### if

```
<if test="表达式">
	语句
</if>
```

### choose

```xml
<choose>
	<when test="表达式">
		语句
	</when>
	……
	<otherwise>
		语句
	</otherwise>
</choose>
```

### where

<where> 元素的作用是会在写入 <where> 元素的地方输出一个 where 语句，另外一个好处是不需要考虑 <where> 元素里面的条件输出是什么样子的，MyBatis 将智能处理。如果所有的条件都不满足，那么 MyBatis 就会查出所有的记录，如果输出后是以 and 开头的，MyBatis 会把第一个 and 忽略。

当然如果是以 or 开头的，MyBatis 也会把它忽略；此外，在 <where> 元素中不需要考虑空格的问题，MyBatis 将智能加上。

```xml
<select id="selectUserByWhere" resultType="com.po.MyUser" parameterType="com.po.MyUser">
    select * from user
    <where>
        <if test="uname != null and uname ! = ''">
            and uname like concat('%',#{uname},'%')
        </if>
        <if test="usex != null and usex != '' ">
            and usex=#{usex}
        </if >
    </where>
</select>
```

### trim

<trim> 元素的主要功能是可以在自己包含的内容前加上某些前缀，也可以在其后加上某些后缀，与之对应的属性是 prefix 和 suffix。

可以把包含内容的首部某些内容覆盖，即忽略，也可以把尾部的某些内容覆盖，对应的属性是 prefixOverrides 和 suffixOverrides。正因为 <trim> 元素有这样的功能，所以也可以非常简单地利用 <trim> 来代替 <where> 元素的功能。

```xml
<!--使用trim元素根据条件动态查询用户信息-->
<select id="selectUserByTrim" resultType="com.po.MyUser"parameterType="com.po.MyUser">
    select * from user
    <trim prefix="where" prefixOverrides = "and | or">
        <if test="uname!=null and uname!=''">
            and uname like concat('%',#{uname},'%')
        </if>
        <if test="usex!=null and usex!=''">
            and usex=#{usex}
        </if>
    </trim>
</select>
```

### set

MyBatis在生成update语句时若使用if标签，如果前面的if没有执行，则可能导致有多余逗号的错误。

使用set标签可以将动态的配置SET 关键字，和剔除追加到条件末尾的任何不相关的逗号。 
没有使用if标签时，如果有一个参数为null，都会导致错误

update xxx set username=?,age=?,

```xml
<update id="updateUserBySet" parameterType="com.po.MyUser">     
    update user
    <set>
        <if test="uname!=null">uname=#{uname}</if>
        <if test="usex!=null">usex=#{usex}</if>
    </set>
    where uid=#{uid}
</update>
```



### froeach

<foreach> 元素主要用在构建 in 条件中，它可以在 SQL 语句中迭代一个集合。

<foreach> 元素的属性主要有 item、index、collection、open、separator、close。

- item 表示集合中每一个元素进行迭代时的别名。
- index 指定一个名字，用于表示在迭代过程中每次迭代到的位置。
- open 表示该语句以什么开始。
- separator 表示在每次进行迭代之间以什么符号作为分隔符。
- close 表示以什么结束。

在使用 <foreach> 元素时，最关键、最容易出错的是 collection 属性，该属性是必选的，但在不同情况下该属性的值是不一样的，主要有以下 3 种情况：

- 如果传入的是单参数且参数类型是一个 List，collection 属性值为 list。
- 如果传入的是单参数且参数类型是一个 array 数组，collection 的属性值为 array。
- 如果传入的参数是多个，需要把它们封装成一个 Map，当然单参数也可以封装成 Map。Map 的 key 是参数名，collection 属性值是传入的 List 或 array 对象在自己封装的 Map 中的 key。

in (3,7,9)

```xml
<select id="selectUserByForeach" resultType="com.po.MyUser" parameterType=
"List">
    select * from user where uid in
    <foreach item="item" index="index" collection="list"
    open="(" separator="," close=")">
        # {item}
    </foreach>
</select>
```

### bind

在进行模糊查询时，如果使用“${}”拼接字符串，则无法防止 SQL 注入问题。如果使用字符串拼接函数或连接符号，但不同数据库的拼接函数或连接符号不同。

例如 mysql的 concat 函数、Oracle 的连接符号“||”，这样 SQL 映射文件就需要根据不同的数据库提供不同的实现，显然比较麻烦，且不利于代码的移植。幸运的是，MyBatis 提供了 <bind> 元素来解决这一问题。

```xml
<select id="selectUserByBind" resultType="com.po.MyUser" parameterType= "com.po.MyUser">
    <!-- bind 中的 uname 是 com.po.MyUser 的属性名-->
    <bind name="paran_uname" value="'%' + uname + '%'"/>
        select * from user where uname like #{paran_uname}
</select>
```

## resultMap

结果集映射，定义映射规则，级联规则和类型转换器

```xml
<resultMap id="" type="">
    <constructor><!-- 类再实例化时用来注入结果到构造方法 -->
        <idArg/><!-- ID参数，结果为ID -->
        <arg/><!-- 注入到构造方法的一个普通结果 -->  
    </constructor>
    <id/><!-- 用于表示哪个列是主键 -->
    <result/><!-- 注入到字段或JavaBean属性的普通结果 -->
    <association property=""/><!-- 用于一对一关联 -->
    <collection property=""/><!-- 用于一对多、多对多关联 -->
    <discriminator javaType=""><!-- 使用结果值来决定使用哪个结果映射 -->
        <case value=""/><!-- 基于某些值的结果映射 -->
    </discriminator>
</resultMap>
```

### 一对一关联查询

在 <association> 元素中通常使用以下属性。

- property：指定映射到实体类的对象属性。
- column：指定表中对应的字段（即查询返回的列名）。
- javaType：指定映射到实体对象属性的类型。
- select：指定引入嵌套查询的子 SQL 语句，该属性用于关联映射中的嵌套查询。

联想：数据库是user_name的形式，实体类是驼峰命名，有几种方式可以自动使二者匹配

场景：查询学生表，包含班级信息

1. 查询两次：学生表查询学生信息，返回一个resultMap,在map中再次查询班级表

2. 执行一个sql语句，嵌套结果。注意，一对多里一定要手写字段匹配。

3. 执行一个sql语句，额外创建一个对象包含学生信息和班级信息字段，返回这个对象

   **:假设有一个`User`类，一个`Car`类，一个`User`对应这一辆`Car`,查询`User`信息的同时查询出他的`Car`的信息.**

   第一种方式

   ```xml
   <mapper namespace="com.wantao.dao.UserDao">
   	<resultMap type="com.wantao.bean.User" id="map1">
   		<id column="user_id" property="userId"></id>
   		<result column="user_name" property="userName" />
   		<result column="c_id" property="cId" />
   		<!--Car相关的属性加上了car.-->
   		<result column="car_id" property="car.carId" />
   		<result column="car_name" property="car.carName" />
   	</resultMap>
   	<select id="findUserById" resultMap="map1">
   		select
   		u.user_id,u.user_name,u.c_id,c.car_id ,c.car_name
   		from tb_user u
   		left join tb_car c on u.c_id = c.car_id
   		where u.user_id=#{id}
   	</select>
   </mapper>
   ```

   第二种方式

   ```xml
   <mapper namespace="com.wantao.dao.UserDao">
   	<resultMap type="com.wantao.bean.User" id="map1">
   		<id column="user_id" property="userId"></id>
   		<result column="user_name" property="userName" />
   		<result column="c_id" property="cId" />
   		<!--Car相关的属性加上了car.-->
   		<result column="car_id" property="car.carId" />
   		<result column="car_name" property="car.carName" />
   	</resultMap>
   	<select id="findUserById" resultMap="map1">
   		select
   		u.user_id,u.user_name,u.c_id,c.car_id ,c.car_name
   		from tb_user u
   		left join tb_car c on u.c_id = c.car_id
   		where u.user_id=#{id}
   	</select>
   </mapper>
   ```

   

### 一对多关联查询

<collection>

同上

场景：一个用户可以下多个订单，一个学生有多次考试成绩

==fetchType属性==：

- eager表示立即加载，及查询某一对象时会立即执行其关联的一对多关系的sql语句
- lazy表示懒加载，不会立即发送，而是确定用到的时候才发送，主要是用来提高性能。

使用懒加载还需要在mybatis.xml中添加如下两行

```xml
   <setting name= "lazyLoadingEnabled" value= "true"/>
    <!--将积极加载改为按需加载-->
    <setting name="aggressiveLazyLoading" value="false"/>
```

## 注解

- @Insert

- @Update

- @Delete

- @Select

- @Result

- @Results

- @ResultMap

- @One

- @Many

- @CacheNamespace

- @Param

- @Options

例子

```java
@Insert("insert into test_class values(null,#{name},#{address})")
    public void insert(@Param("name") String name, @Param("address") String address);
    //参数列表要和sql语句参数顺序一致，加上@Param可以指定填充内容。
```

## 日志

作用：记录程序运行信息

<font color=red>name:</font>
logImpl
<font color=red>value:</font>
SLF4J       LOG4J       LOG4J2      JDK_LOGGING     COMMONS_LOGGING     STDOUT_LOGGING      NO_LOGGING
例如mybatis默认的日志

```xml
<settings >
    <setting name="logImpl" value="STDOUT_LOGGING"/><!-- 标准工厂日志：系统默认的日志，无须添加任何依赖。-->
</settings>
```

常用log4j

- 添加依赖
- 写配置文件
- Logger使用

```per
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
#log4j.appender.stdout.layout.ConversionPattern =  %d{ABSOLUTE} %5p %c{ 1 }:%L - %m%n

### 输出到日志文件 ###
log4j.appender.D = org.apache.log4j.DailyRollingFileAppender
log4j.appender.D.File = logs/log.log
log4j.appender.D.Append = true
log4j.appender.D.Threshold = DEBUG ## 输出DEBUG级别以上的日志
log4j.appender.D.layout = org.apache.log4j.PatternLayout
log4j.appender.D.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
```

## 缓存

开发中通常对数据库查询的性能要求很高，mybatis也提供了缓存机制。分为一级缓存和二级缓存。

### 一级缓存（SqlSession级别）

操作数据库时需要构建SqlSession对象，在对象中有一个HashMap用于存储缓存数据，不同的session对象之间缓存相互不影响。

作用域就是sqlsession范围，当在同一个sqlsession中执行两次相同的sql语句时，第一次执行完毕会将查询结果写到缓存（内存）中，第二次查询时直接从缓存中获取，提高效率。如果该对象执行了增删改操作并提交，则会清空该对象的一级缓存，目的是保证缓存中的数据时最新的，防止脏读。

**怎么判断某两次查询是完全相同的查询？**

　　mybatis认为，对于两次查询，如果以下条件都完全一样，那么就认为它们是完全相同的两次查询。

　　1 传入的statementId

　　2 查询时要求的结果集中的结果范围

  　　3. 这次查询所产生的最终要传递给JDBC java.sql.Preparedstatement的Sql语句字符串（boundSql.getSql() ）

　　4 传递给java.sql.Statement要设置的参数值

<font color=red>**默认开启一级缓存，不需要配置**</font>

map中的key为方法id

### 二级缓存（mapper级别）

多个sqlsession共享，作用域为mapper的同一个namespace。

同样适用HashMap进行数据存储。

SqlSessionFactory层面上的二级缓存默认是不开启的，二级缓存的开席需要进行配置，实现二级缓存的时候，MyBatis要求返回的POJO必须是可序列化的。 也就是要求实现Serializable接口，配置方法很简单，只需要在映射XML文件配置就可以开启缓存了<cache/>，如果我们配置了二级缓存就意味着：

- 映射语句文件中的所有select语句将会被缓存。

- 映射语句文件中的所欲insert、update和delete语句会刷新缓存。

- 缓存会使用默认的Least Recently Used（LRU，最近最少使用的）算法来收回。

- 根据时间表，比如No Flush Interval,（CNFI没有刷新间隔），缓存不会以任何时间顺序来刷新。

- 缓存会存储列表集合或对象(无论查询方法返回什么)的1024个引用

- 缓存会被视为是read/write(可读/可写)的缓存，意味着对象检索不是共享的，而且可以安全的被调用者修改，不干扰其他调用者或线程所做的潜在修改。

  

默认没有开启二级缓存，需要手动开启

```xml
<setting name=cacheEnabled" value="true"/>
```

在映射文件中添加缓存配置

```xml
 <!--开启本mapper的namespace下的二级缓存-->
    <!--
        eviction:代表的是缓存回收策略，目前MyBatis提供以下策略。默认为LRU
        (1) LRU,最近最少使用的，一处最长时间不用的对象
        (2) FIFO,先进先出，按对象进入缓存的顺序来移除他们
        (3) SOFT,软引用，移除基于垃圾回收器状态和软引用规则的对象
        (4) WEAK,弱引用，更积极的移除基于垃圾收集器状态和弱引用规则的对象。这里采用的是LRU，
                移除最长时间不用的对形象

        flushInterval:刷新间隔，单位为毫秒，默认不设置也就是没有刷新间隔，缓存仅在调用语句时刷新

        size:引用数目，一个正整数，代表缓存最多可以存储多少个对象，不宜设置过大。设置过大会导致内存溢出。
        这里配置的是1024个对象

        readOnly:只读，意味着缓存数据只能读取而不能修改，这样设置的好处是我们可以快速读取缓存，缺点是我们没有办法修改缓存，他的默认值是false，不允许我们修改
    -->
<cache eviction="LRU" flushInterval="100000" readOnly="true" size="1024"/>  
    <!--可以通过设置useCache来规定这个sql是否开启缓存，ture是开启，false是关闭-->
		<select id="selectAllStudents" resultMap="studentMap" useCache="true">
        SELECT id, name, age FROM student
    </select>
    <!--刷新二级缓存-->
    <select id="selectAllStudents" resultMap="studentMap" flushCache="true">
        SELECT id, name, age FROM student
    </select>

```

​    

**<font color=red>当两级都开启是，会优先从二级缓存找，然后找一级</font>**

### 一级缓存和二级缓存的作用：

1）一级缓存: 基于 PerpetualCache 的 HashMap 本地缓存，其存储作用域为 Session，当 Session flush 或 close 之后，该 Session 中的所有 Cache 就将清空，默认打开一级缓存。

（2）二级缓存与一级缓存其机制相同，默认也是采用 PerpetualCache，HashMap 存储，不同在于其存储作用域为 Mapper(Namespace)，并且可自定义存储源，如 Ehcache。默认不打开二级缓存，要开启二级缓存，使用二级缓存属性类需要实现**Serializable**序列化接口(可用来保存对象的状态)，可在它的映射文件中配置 cache  ；

（3）对于缓存数据更新机制，当某一个作用域(一级缓存 Session/二级缓存Namespaces)的进行了Inser/Update/Delete操作后，默认该作用域下所有 select 中的缓存将被 clear

## 配置文件Mybatis-config.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration><!-- 配置 -->
    <properties /><!-- 属性 -->
    <settings /><!-- 设置 -->
    <typeAliases /><!-- 类型命名 -->
    <typeHandlers /><!-- 类型处理器 -->
    <objectFactory /><!-- 对象工厂 -->
    <plugins /><!-- 插件 -->
    <environments><!-- 配置环境 -->
        <environment><!-- 环境变量 -->
            <transactionManager /><!-- 事务管理器 -->
            <dataSource /><!-- 数据源 -->
        </environment>
    </environments>
    <databaseIdProvider /><!-- 数据库厂商标识 -->
    <mappers /><!-- 映射器 -->
</configuration>
```

**以上这些配置项不能颠倒，可以不写某个配置，但是前后次序不能乱，否则启动会有异常。**

### properties

properties 属性可以给系统配置一些运行参数，可以放在 XML 文件或者 properties 文件中，而不是放在 编码中，这样的好处在于方便参数修改，而不会引起代码的重新编译。一般而言，MyBatis 提供了 3 种方式让我们使用 properties，它们是：

- property 子元素。
- properties 文件。
- 程序代码传递。



1. #### property子元素

在配置文件中使用<properties>的子元素<property> 定义需要的属性配置，然后再需要该属性的地方用${属性名}即可调用该属性

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties>
        <property name="driver" value="com.mysql.jdbc.Driver" />
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf8" />
        <property name="username" value="root" />
        <property name="password" value="1128" />
    </properties>
    <!--数据库环境-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="${driver}" />
                <property name="url"    value="${url}" />
                <property name="username" value="${username}" />
                <property name="password" value="${password}" />
            </dataSource>
        </environment>
    </environments>
    <!-- 映射文件 -->
    <mappers>
        <mapper resource="com/mybatis/mapper/RoleMapper.xml" />
    </mappers>
</configuration>
```

2. #### properties文件

新建一个jdbc.properties文件用来保存数据库信息，如下：

```xml
driver=""
url=""
name=""
password=""
```

然后在xml文件中引入该配置，用到<properties>的resource属性,使用时也是${属性名}来调用，这样做的好处在于以后需要更改数据库配置信息的时候只需要维护一个properties文件即可，不用更改xml文件。需要注意的是，在引入文件后，<property>给同一变量赋值，引用的时候会引本次赋值的结果而不是文件中的结果

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="jdbc.properties">
        <property name="username" value="root" />
        <property name="password" value="1128" />
    </properties>
    <!--数据库环境-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="${driver}" />
                <property name="url"    value="${url}" />
                <property name="username" value="${username}" />
                <property name="password" value="${password}" />
            </dataSource>
        </environment>
    </environments>
    <!-- 映射文件 -->
    <mappers>
        <mapper resource="com/mybatis/mapper/RoleMapper.xml" />
    </mappers>
</configuration>
```

3. #### 使用代码来传递

在真实的生产环境中，数据库的用户密码是对开发人员和其他人员保密的。运维人员为了保密，一般都需要把用户和密码经过加密成为密文后，配置到 properties 文件中。

对于开发人员及其他人员而言，就不知道其真实的用户密码了，数据库也不可能使用已经加密的字符串去连接，此时往往需要通过解密才能得到真实的用户和密码了。

现在假设系统已经为提供了这样的一个 CodeUtils.decode（str）进行解密，那么我们在创建 SqlSessionFactory 前，就需要把用户名和密码解密，然后把解密后的字符串重置到 properties 属性中，如下所示。

```java
String resource = "mybatis-config.xml";
InputStream inputStream;
Inputstream in = Resources.getResourceAsStream("jdbc.properties");
Properties props = new Properties();
props.load(in);
String username = props.getProperty("database.username");
String password = props.getProperty("database.password");
//解密用户和密码，并在属性中重置
props.put("database.username", CodeUtils.decode(username));
props.put ("database.password", CodeUtils.decode(password)); 
inputstream = Resources.getResourceAsStream(resource);
//使用程序传递的方式覆盖原有的properties属性参数
SqlSessionFactory = new SqlSessionFactoryBuilder().build(inputstream, props);
```

### settings

在 MyBatis 中 settings 是最复杂的配置，它能深刻影响 MyBatis 底层的运行，但是在大部分情况下使用默认值便可以运行，所以在大部分情况下不需要大量配置它，只需要修改一些常用的规则即可，比如自动映射、驼峰命名映射、级联规则、是否启动缓存、执行器（Executor）类型等。



| 配置项                            | 作用                                                         | 配置选项                                                     | 默认值                                                       |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **[cacheEnabled]()**                       | 该配置影响所有映射器中配置缓存的全局开关                     | true\|false                                                  | true                                                         |
| **[lazyLoadingEnabled]()**                 | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。在特定关联关系中可通过设置 fetchType 属性来覆盖该项的开关状态 | true\|false                                                  | false                                                        |
| aggressiveLazyLoading             | 当启用时，对任意延迟属性的调用会使带有延迟加载属性的对象完整加载；反之，每种属性将会按需加载 | true\|felse                                                  | 版本3.4.1 （不包含） 之前 true，之后 false                   |
| multipleResultSetsEnabled         | 是否允许单一语句返回多结果集（需要兼容驱动）                 | true\|false                                                  | true                                                         |
| useColumnLabel                    | 使用列标签代替列名。不同的驱动会有不同的表现，具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果 | true\|false                                                  | true                                                         |
| useGeneratedKeys                  | 允许JDBC 支持自动生成主键，需要驱动兼容。如果设置为 true，则这个设置强制使用自动生成主键，尽管一些驱动不能兼容但仍可正常工作（比如 Derby） | true\|false                                                  | false                                                        |
| autoMappingBehavior               | 指定 MyBatis 应如何自动映射列到字段或属性。 NONE 表示取消自动映射。 PARTIAL 表示只会自动映射，没有定义嵌套结果集和映射结果集。 FULL 会自动映射任意复杂的结果集（无论是否嵌套） | NONE、PARTIAL、FULL                                          | PARTIAL                                                      |
| autoMappingUnkno wnColumnBehavior | 指定自动映射当中未知列（或未知属性类型）时的行为。 默认是不处理，只有当日志级别达到 WARN 级别或者以下，才会显示相关日志，如果处理失败会抛出 SqlSessionException 异常 | NONE、WARNING、FAILING                                       | NONE                                                         |
| defaultExecutorType               | 配置默认的执行器。SIMPLE 是普通的执行器；REUSE 会重用预处理语句（prepared statements）；BATCH 执行器将重用语句并执行批量更新 | SIMPLE、REUSE、BATCH                                         | SIMPLE                                                       |
| defaultStatementTimeout           | 设置超时时间，它决定驱动等待数据库响应的秒数                 | 任何正整数                                                   | Not Set (null)                                               |
| defaultFetchSize                  | 设置数据库驱动程序默认返回的条数限制，此参数可以重新设置     | 任何正整数                                                   | Not Set (null)                                               |
| safeRowBoundsEnabled              | 允许在嵌套语句中使用分页（RowBounds）。如果允许，设置 false  | true\|false                                                  | false                                                        |
| safeResultHandlerEnabled          | 允许在嵌套语句中使用分页（ResultHandler）。如果允许，设置false | true\|false                                                  | true                                                         |
| **[mapUnderscoreToCamelCase]()**           | 是否开启自动驼峰命名规则映射，即从经典数据库列名 A_COLUMN 到经典 [Java](http://c.biancheng.net/java/) 属性名 aColumn 的类似映射 | true\|false                                                  | false                                                        |
| **[localCacheScope]()**                    | MyBatis 利用本地缓存机制（Local Cache）防止循环引用（circular references）和加速联复嵌套査询。 默认值为 SESSION，这种情况下会缓存一个会话中执行的所有查询。若设置值为 STATEMENT，本地会话仅用在语句执行上，对相同 SqlScssion 的不同调用将不会共享数据 | SESSION\|STATEMENT                                           | SESSION                                                      |
| jdbcTypeForNull                   | 当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型。某些驱动需要指定列的 JDBC 类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR 或 OTHER | NULL、VARCHAR、OTHER                                         | OTHER                                                        |
| lazyLoadTriggerMethods            | 指定哪个对象的方法触发一次延迟加载                           | —                                                            | equals、clone、hashCode、toString                            |
| defaultScriptingLanguage          | 指定动态 SQL 生成的默认语言                                  | —                                                            | org.apache.ibatis .script.ing.xmltags .XMLDynamicLanguageDriver |
| callSettersOnNulls                | 指定当结果集中值为 null 时，是否调用映射对象的 setter（map 对象时为 put）方法，这对于 Map.kcySet() 依赖或 null 值初始化时是有用的。注意，基本类型（int、boolean 等）不能设置成 null | true\|false                                                  | false                                                        |
| **[logPrefix]()**                          | 指定 MyBatis 增加到日志名称的前缀                            | 任何字符串                                                   | Not set                                                      |
| **[logImpl]()**                       | 指定 MyBatis 所用日志的具体实现，未指定时将自动査找          | SLF4J\|LOG4J\|LOG4J2\|JDK_LOGGING \|COMMONS_LOGGING \|ST DOUT_LOGGING\|NO_LOGGING | Not set                                                      |
| **[proxyFactory]()**                      | 指定 MyBatis 创建具有延迟加栽能力的对象所用到的代理工具      | CGLIB\|JAVASSIST                                             | JAVASSIST （MyBatis 版本为 3.3 及以上的）                    |
| vfsImpl                           | 指定 VFS 的实现类                                            | 提供 VFS 类的全限定名，如果存在多个，可以使用逗号分隔        | Not set                                                      |
| useActualParamName                | 允许用方法参数中声明的实际名称引用参数。要使用此功能，项目必须被编译为 Java 8 参数的选择。（从版本 3.4.1 开始可以使用） | true\|false                                                  | true                                                         |



这里给出一个全量的配置案例

```xml
<settings>
    <setting name="cacheEnabled" value="true"/>
    <setting name="lazyLoadingEnabled" value="true"/>
    <setting name="multipleResultSetsEnabled" value="true"/>
    <setting name="useColumnLabel" value="true"/>
    <setting name="useGeneratedKeys" value="false"/>
    <setting name="autoMappingBehavior" value="PARTIAL"/>
    <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
    <setting name="defaultExecutorType" value="SIMPLE"/>
    <setting name="defaultStatementTimeout" value="25"/>
    <setting name="defaultFetchSize" value="100"/>
    <setting name="safeRowBoundsEnabled" value="false"/>
    <setting name="mapUnderscoreToCamelCase" value="false"/>
    <setting name="localCacheScope" value="SESSION"/>
    <setting name="jdbcTypeForNull" value="OTHER"/>
    <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```



### typeAliases

由于类的全限定名称很长，需要大量使用的时候，总写那么长的名称不方便。在 MyBatis 中允许定义一个简写来代表这个类，这就是别名，别名分为系统定义别名和自定义别名。

例如：

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```

在之后使用时就可以用别名来代替类全称了，简化了使用

以上述配置为例，当用到的这些类都在同一个包下时，也可直接扫描该包而不是一个一个设置。该包下的每个类的类名首字母小写就是该类的别名

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

当然，不想自动用类名做别名时也可以通过注解指定别名

```java
@Alias("author")
public class Author {
    ...
}
```



下面是一些为常见的 Java 类型内建的类型别名。它们都是不区分大小写的，注意，为了应对原始类型的命名重复，采取了特殊的命名风格。

| 别名       | [Java](http://c.biancheng.net/java/) 类型 | 是否支持数组 |
| ---------- | ----------------------------------------- | ------------ |
| _byte      | byte                                      | 是           |
| _long      | long                                      | 是           |
| _short     | short                                     | 是           |
| _int       | int                                       | 是           |
| _integer   | int                                       | 是           |
| _double    | double                                    | 是           |
| _float     | float                                     | 是           |
| _boolean   | boolean                                   | 是           |
| string     | String                                    | 是           |
| byte       | Byte                                      | 是           |
| long       | Long                                      | 是           |
| short      | Short                                     | 是           |
| int        | Integer                                   | 是           |
| integer    | Integer                                   | 是           |
| double     | Double                                    | 是           |
| float      | Float                                     | 是           |
| boolean    | Boolean                                   | 是           |
| date       | Date                                      | 是           |
| decimal    | BigDecimal                                | 是           |
| bigdecimal | BigDecimal                                | 是           |
| object     | Object                                    | 是           |
| map        | Map                                       | 否           |
| hashmap    | HashMap                                   | 否           |
| list       | List                                      | 否           |
| arraylist  | ArrayList                                 | 否           |
| collection | Collection                                | 否           |
| iterator   | Iterator                                  | 否           |
| ResultSet  | ResultSet                                 | 否           |



如果需要使用对应类型的数组型，要看其是否能支持数据，如果支持只需要使用别名加`[]`即可，比如 _int 数组的别名就是 _int[]。而类似 list 这样不支持数组的别名，则不能那么写。

有时候要通过代码来实现注册别名，在 MyBatis 中别名由类 TypeAliasRegistry（org.apache.ibatis.type.TypeAliasRegistry）去定义,以下为源码

```java
    
public TypeAliasRegistry() {
    registerAlias("string", String.class);

    registerAlias("byte", Byte.class);
    registerAlias("long", Long.class);
    ......
    registerAlias("byte[]",Byte[].class); registerAlias("long[]",Long[].class);
    ......
    registerAlias("map", Map.class);
    registerAlias("hashmap", HashMap.class);
    registerAlias("list", List.class); registerAlias("arraylist", ArrayList.class);
    registerAlias("collection", Collection.class);
    registerAlias("iterator", Iterator.class);
    registerAlias("ResultSet", ResultSet.class);
}

```

所以使用 TypeAliasRegistry 的 registerAlias 方法就可以注册别名了。一般是通过 Configuration 获取 TypeAliasRegistry 类对象，其中有一个 getTypeAliasRegistry 方法可以获得别名，如 configuration.getTypeAliasRegistry()。

然后就可以通过 registerAlias 方法对别名注册了。而事实上 Configuration 对象也对一些常用的配置项配置了别名，如下所示。

```java
//事务方式别名
typeAliasRegistry.registerAlias("JDBC",JdbcTransactionFactory.class);
typeAliasRegistry.registerAlias("MANAGED",ManagedTransactionFactory.class);
//数据源类型别名
typeAliasRegistry.registerAlias("JNDI",JndiDataSourceFactory.class);
typeAliasRegistry.registerAlias("POOLED",
PooledDataSourceFactory.class);
typeAliasRegistry.registerAlias("UNPOOLED",UnpooledDataSourceFactory.class);
//缓存策略别名
typeAliasRegistry.registerAlias("PERPETUAL",PerpetualCache.class);
typeAliasRegistry.registerAlias("FIFO",FifoCache.class);
typeAliasRegistry.registerAlias("LRU",LruCache.class); typeAliasRegistry.registerAlias("SOFT", SoftCache.class); typeAliasRegistry.registerAlias("WEAK", WeakCache.class);
//数据库标识别名
typeAliasRegistry.registerAlias("DB_VENDOR",
VendorDatabaseIdProvider.class);
//语言驱动类别名
typeAliasRegistry.registerAlias("XML",XMLLanguageDriver.class);
typeAliasRegistry.registerAlias("RAW",RawLanguageDriver.class);
//日志类别名
typeAliasRegistry.registerAlias("SLF4J", Slf4jImpl.class);
typeAliasRegistry.registerAlias("COMMONS_LOGGTNG",JakartmCommonsLogginglmpl.class);
typeAliasRegistry.registerAlias("LOG4J", Log4jImpl.class);
typeAliasRegistry.registerAlias("LOG4J2", Log4j2Impl.class);
typeAliasRegistry.registerAlias("JDK_LOGGING", Jdk14LoggingImpl.class);
typeAliasRegistry.registerAlias("STDOUT_LOGGING", StdOutImpl.class);
typeAliasRegistry.registerAlias("NO_LOGGING",NoLoggingImpl.class);
//动态代理别名
typeAliasRegistry.registerAlias("CGLIB",CglibProxyFactory.class);
typeAliasRegistry.registerAlias("JAVASSIST",JavassistProxyFactory.class);
```

### typeHandler
### objectFactory

### plugins

### enviroments

```xml
<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC" /> <!-- 选择用哪种方式来处理事务 -->
        <dataSource type="POOLED">     <!-- 选择连接数据库的方式，这里选择的是连接池 -->
            <property name="driver" value="${database.driver}" />
            <property name="url"
                value="${database.url}" />
            <property name="username" value="${database.username}" />
            <property name="password" value="${database.password}" />
        </dataSource>
    </environment>
</environments>
```

主要用来配置数据库信息，内部可以配置多个<environment>，相当于多个数据库环境，id即为每个数据库环境的标识，通过<environments>里的default属性来决定使用哪个环境。

#### transactionManager事务管理器

可配置成以下两种方式

```xml
<transactionManager type="JDBC"/>
<transactionManager type="MANAGED"/>
```

JDBC 使用 JdbcTransactionFactory 生成的 JdbcTransaction 对象实现。它是以 JDBC 的方式对数据库的提交和回滚进行操作。

MANAGED 使用 ManagedTransactionFactory 生成的 ManagedTransaction 对象实现。它的提交和回滚方法不用任何操作，而是把事务交给容器处理。在默认情况下，它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置为 false 来阻止它默认的关闭行为。

源码：

在 MyBatis 中，transactionManager 提供了两个实现类，它需要实现接口 Transaction（org.apache.ibatis.transaction.Transaction），它的定义代码如下所示。

```java
public interface Transaction {
    Connection getConnection() throws SQLException;

    void commit() throws SQLException;

    void rollback() throws SQLException;

    void close() throws SQLException;

    Integer getTimeout() throws SQLException;
}
```

从方法可知，它主要的工作就是提交（commit）、回滚（rollback）和关闭（close）数据库的事务。MyBatis 为 Transaction 提供了两个实现类：JdbcTransaction 和 ManagedTransaction

![Transaction的实现类](http://c.biancheng.net/uploads/allimg/190710/5-1ZG010033A61.png)



如果不想使用mybatis采用的规则时，可以自己定义。

实现一个自定义事务工厂：创建类，实现TransactionFactory接口

```java
public class MyTransactionFactory implements TransactionFactory {
    @Override
    public void setProperties(Properties props) {
    }
    @Override
    public Transaction newTransaction(Connection conn) {
        return new MyTransaction(conn);
    }
    @Override
    public Transaction newTransaction(DataSource dataSource, TransactionlsolationLevel level, boolean autoCommit) {
        return new MyTransaction(dataSource, level, autoCommit);
    }
}
```

这里就实现了 TransactionFactory 所定义的工厂方法，这个时候还需要事务实现类 MyTransaction，它用于实现 Transaction 接口

```java
public class MyTransaction extends JdbcTransaction implements Transaction {

    public MyTransaction(DataSource ds, TransactionIsolationLevel desiredLevel,
            boolean desiredAutoCommit) {
        super(ds, desiredLevel, desiredAutoCommit);
    }

    public MyTransaction(Connection connection) {
        super(connection);
    }

    public Connection getConnection() throws SQLException {
        return super.getConnection();
    }

    public void commit() throws SQLException {
        super.commit();
    }

    public void rollback() throws SQLException {
        super.rollback();
    }

    public void close() throws SQLException {
        super.close();
    }

    public Integer getTimeout() throws SQLException {
        return super.getTimeout();
    }
}
```



#### dataSource 数据源

数据源配置有三种

```xml
<dataSource type="UNPOOLED">
<dataSource type="POOLED">
<dataSource type="JNDI">
```

在 MyBatis 中，数据库通过 PooledDataSource Factory、UnpooledDataSourceFactory 和 JndiDataSourceFactory 三个工厂类来提供，前两者对应产生 PooledDataSource、UnpooledDataSource 类对象，而 JndiDataSourceFactory 则会根据 JNDI 的信息拿到外部容器实现的数据库连接对象。

1. UNPOOLED 采用非数据库池的管理方式，每次请求都会打开一个新的数据库连接，所以创建会比较慢。在一些对性能没有很高要求的场合可以使用它。

   对有些数据库而言，使用连接池并不重要，那么它也是一个比较理想的选择。UNPOOLED 类型的数据源可以配置以下几种属性：

   - driver 数据库驱动名，比如 [MySQL](http://c.biancheng.net/mysql/) 的 com.mysql.jdbc.Driver。
   - url 连接数据库的 URL。
   - username 用户名。
   - password 密码。
   - defaultTransactionIsolationLevel 默认的连接事务隔离级别


   传递属性给数据库驱动也是一个可选项，注意属性的前缀为“driver.”，例如 driver.encoding=UTF8。它会通过 DriverManager.getConnection（url,driverProperties）方法传递值为 UTF8 的 encoding 属性给数据库驱动。

2. 数据源 POOLED 利用“池”的概念将 JDBC 的 Connection 对象组织起来，它开始会有一些空置，并且已经连接好的数据库连接，所以请求时，无须再建立和验证，省去了创建新的连接实例时所必需的初始化和认证时间。它还控制最大连接数，避免过多的连接导致系统瓶颈。

   除了 UNPOOLED 下的属性外，会有更多属性用来配置 POOLED 的数据源

| 名称                          | 说明                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| poolMaximumActiveConnections  | 是在任意时间都存在的活动（也就是正在使用）连接数量，默认值为 10 |
| poolMaximumIdleConnections    | 是任意时间可能存在的空闲连接数                               |
| poolMaximumCheckoutTime       | 在被强制返回之前，池中连接被检出（checked out）的时间，默认值为 20 000 毫秒（即 20 秒） |
| poolTimeToWait                | 是一个底层设置，如果获取连接花费相当长的时间，它会给连接池打印状态日志，并重新尝试获取一个连接（避免在误配置的情况下一直失败），默认值为 20 000 毫秒（即 20 秒）。 |
| poolPingQuery                 | 为发送到数据库的侦测查询，用来检验连接是否处在正常工作秩序中，并准备接受请求。默认是“NO PING QUERY SET”，这会导致多数数据库驱动失败时带有一个恰当的错误消息。 |
| poolPingEnabled               | 为是否启用侦测查询。若开启，也必须使用一个可执行的 SQL 语句设置 poolPingQuery 属性（最好是一个非常快的 SQL），默认值为 false。 |
| poolPingConnectionsNotUsedFor | 为配置 poolPingQuery 的使用频度。这可以被设置成匹配具体的数据库连接超时时间，来避免不必要的侦测，默认值为 0（即所有连接每一时刻都被侦测——仅当 poolPingEnabled 为 true 时适用）。 |

3. 数据源 JNDI 的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。这种数据源配置只需要两个属性：

   1）initial_context

   用来在 InitialContext 中寻找上下文（即，initialContext.lookup（initial_context））。initial_context 是个可选属性，如果忽略，那么 data_source 属性将会直接从 InitialContext 中寻找。

   2）data_source

   是引用数据源实例位置上下文的路径。当提供 initial_context 配置时，data_source 会在其返回的上下文中进行查找；当没有提供 initial_context 时，data_source 直接在 InitialContext 中查找。

   与其他数据源配置类似，它可以通过添加前缀“env.”直接把属性传递给初始上下文（InitialContext）。比如 env.encoding=UTF8，就会在初始上下文实例化时往它的构造方法传递值为 UTF8 的 encoding 属性。

### mappers 映射器

既然 MyBatis 的行为已经由上述元素配置完了，我们现在就要来定义 SQL 映射语句了。 但首先，我们需要告诉 MyBatis 到哪里去找到这些语句。 在自动查找资源方面，Java 并没有提供一个很好的解决方案，所以最好的办法是直接告诉 MyBatis 到哪里去找映射文件。 你可以使用相对于类路径的资源引用，或完全限定资源定位符（包括 `file:///` 形式的 URL），或类名和包名等。例如：

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

### 鉴别器

<discriminator>

作用：根据查询到的字段，不同值再分别走不同的resultmap

场景：例如做一个人事管理系统，系统管理员、经理、普通员工、临时工等都可以存到同一张表里，比如user表，用字段role来分别权限，比如1位管理员，2为普通员工等。每个身份需要保存的信息大体上肯定是一样的，比如姓名，身份证，年龄等，只是有些细微差别例如经理需要保存他管理哪个部门，普通员工需要记录他隶属于哪个部门，管理员需要保存他有哪些权限等。因此可以在一张表里存储所有信息，编写业务方法时，创建一个抽象的person类来对应该表，然后分别建不同身份的子类基础person父类，每个子类里只用写自己独有的字段即可。这样做的话在创建对象时就需要先查出该用户的权限，然后再写if，根据不通权限new不同对象。鉴别器的作用就是在sql查询阶段帮我们做这个事情。

