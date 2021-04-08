# Spring整合Mybatis

## 1.创建maven项目

## 2.导入依赖

| MyBatis-Spring | MyBatis | Spring 框架 | Spring Batch | Java    |
| -------------- | ------- | ----------- | ------------ | ------- |
| 2.0            | 3.5+    | 5.0+        | 4.0+         | Java 8+ |
| 1.3            | 3.4+    | 3.2.2+      | 2.1+         | Java 6+ |

> 1、spring的jar包 	2、Mybatis的jar包 	
>
> 3、Spring+mybatis的整合包。 4、Mysql的数据库驱动jar包。 
>
> 5、数据库连接池的jar包。

```xml
		<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.9.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.5</version>
        </dependency>
     <!--这里使用jdbc方式连接，所以导入jdbc包。如果需要用连接池，就导入对应连接池的包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.9.RELEASE</version>
        </dependency>
  		<!--junit测试-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
				<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.2.9.RELEASE</version>
        </dependency>
		<!-- 切面-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.6</version>
        </dependency>
 <!-- mybatis依赖 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.1</version>
        </dependency>
        <!-- mysql依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.21</version>
        </dependency>
```

## 3.搭建Mybatis项目，进行测试

### 3.1配置Mybatis-config.xml

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
      <!--数据库环境--><!--和Spring整合后 environments 配置将废除 -->
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
          <package name="com.demo.mapper"/>
      </mappers>
  </configuration>
  ```

###   3.2编写pojo文件

```java
public class Admin {
    private String name;
    private int age;
	//此处省略set、get、 toString方法
}
```

### 3.3编写mapper文件

#### AdminMapper.java

```java
public interface AdminMapper {
    Admin findById(int id);
}
```

#### AdminMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.demo.mapper.AdminMapper">
    <select id="findById" parameterType="int" resultType="com.demo.pojo.Admin">
        select * from admin where id = #{id}
    </select>
</mapper>
```

### 3.4编写测试类

```java
public class Tests {
    public static void main(String[] args) throws IOException {
        InputStream in= Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactory factory= new SqlSessionFactoryBuilder().build(in);
        SqlSession session=factory.openSession();
        AdminMapper mapper=session.getMapper(AdminMapper.class);
        System.out.println(mapper.findById(1));
		}
}
```

**测试成功后，进行整合Spring**

## 4.Spring整合

### 4.1.引入applicationContext. Xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
        ">

    

</beans>
```

### 4.2使用spring的数据源替换Mybatis的数据源配置

> 在spring配置文件中加入以下代码，配置数据源信息

```xml
  <context:property-placeholder location="classpath:db.properties"/>
  <bean id="driverManager"
        class="org.springframework.jdbc.datasource.DriverManagerDataSource">
            <property name="driverClassName" value="${jdbc.password}"/>
            <property name="url" value="${jdbc.url}"/>
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
  </bean>
```

db.properties文件

```properites
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/xsh?characterEncoding=utf-8
jdbc.username=root
jdbc.password=Logan
```

<font color="red">此时，就可以删除mybatis核心配置文件mybatis-config.xml中的数据源配置</font>
![image-20201028090209736](/Users/beifeng/Documents/笔记/笔记图片/image-20201028090209736.png)

### 4.3通过spring创建SqlSessionFactory

- 在 **MyBatis** 中，是通过 `SqlSessionFactoryBuilder` 来创建 `SqlSessionFactory`
- 而在 **MyBatis-Spring** 中，则使用 `SqlSessionFactoryBean` 来创建

```xml
 <!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="driverManager"/>
        <!--绑定mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/demo/mapper/*.xml"/>
        <!--注册Mapper.xml映射器-->
    </bean>
```

唯一的必要属性：用于 JDBC 的<font color="red">DataSource</font>，注入上述创建好的<font color="red">数据源</font>

这里加入两个属性配置：

- `configLocation`：指定mybatis的xml配置文件路径
- `mapperLocations`：注册Mapper.xm映射器

这里，我们在spring配置文件中注册了<font color="red">Mapper.xml</font>，就可以删除<font color="red">mybatis-config.xml</font>中的<font color="red">Mapper.xml</font>的注册

<img src="/Users/beifeng/Library/Application Support/typora-user-images/image-20201028093410250.png" alt="image-20201028093410250" style="zoom:67%;" />

### 4.4创建sqlSession对象

- 在`mybatis`中，`SqlSession` 提供了在数据库执行 SQL 命令所需的所有方法
- 而在`spring-mybatis`，没有`SqlSession`了，而是`SqlSessionTemplate`，这两个是同一个东西

> 在`spring`配置文件中加入以下代码，创建`sqlSession`

- 这里使用上述创建好的`sqlSessionFactory` 作为构造方法的参数来创建 `SqlSessionTemplate` 对象。

#### 方式一

  ```xml
  <!--SqlSessionTemplate:就是我们使用的sqlSession-->
  <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
      <!--只能使用构造器进行注入，因为没有set方法-->
      <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"/>
  </bean>
  ```

  #### 测试一

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class Tests {
    @Autowired
    SqlSessionTemplate template;
    @Test
    public void test(){
        AdminMapper mapper=template.getMapper(AdminMapper.class);
        System.out.println(mapper.findById(1));
    }
 }
```

#### 方式二

```xml
  <!--SqlSessionTemplate:就是我们使用的sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!--只能使用构造器进行注入，因为没有set方法-->
        <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>

    <!-- =======================分割线======================-->

    <!-- 上面扫描mapper只能扫描xml文件，如果想扫描java文件，用下面的写法-->
    <!-- 扫描并注册单个mapper的java和xml文件   -->
    <bean id="adminMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
        <property name="mapperInterface" value="com.demo.mapper.AdminMapper"/>
    </bean>
    <!--    扫描并注册basePackage 下的mapper中的java和xml文件-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.demo.mapper"/>
    </bean>
```

#### 测试二

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class Tests {
		
    @Autowired
    AdminMapper mapper;
    @Test
    public void test2(){
      //直接使用mapper对象就可以
        System.out.println(mapper.findById(1));
    }
}
```

因为applicationContext注册了mapper包中java文件和xml文件，spring将mapper自动封装。

## 5.Spring事务

编码式



### 方式一：xml

声明式 tx包和约束

要开启 Spring 的事务处理功能，在 Spring 的配置文件中创建一个 DataSourceTransactionManager 对象:

```xml
<!--扫描指定包 开启注解-->
        <context:component-scan base-package="com.demo"/>
        <context:annotation-config/>
<!-- =======================分割线======================-->
    <!--  Spring 事务管理   -->
    <bean id="DataSourceTractionManger"
        class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource"></constructor-arg>
    </bean>
    <!-- 配置事务通知    -->
    <tx:advice id="txAdvice" transaction-manager="DataSourceTractionManger">
        <tx:attributes>
            <tx:method name="transfer" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <!-- 配置事务切入   -->
<!--    execution(* com.sm.service..*Service.*(..))-->
    <aop:config>
        <aop:pointcut id="pc" expression="execution(* com.demo.service..*Service.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pc"/>
    </aop:config>
```

在service包下创建 文件

```java
@Service
public class AdminService {
    @Autowired
    AdminMapper mapper;
    public void transfer(){
        mapper.updateById(1,mapper.findById(1).getAge()+1);
        int i=1/0;
        mapper.updateById(2,mapper.findById(2).getAge()-1);
    }
}
```

### 方式二：annotation

- 配置applicationContext.xml文件

```xml
<!--扫描指定包 开启注解-->
        <context:component-scan base-package="com.demo"/>
        <context:annotation-config/>
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">  
     <property name="dataSource" ref="dataSource"/>  
  </bean>
  <!-- 配置注解事务 -->  
  <tx:annotation-driven transaction-manager="txManager"/>  

```

- 在类中定义事务

```java
//@Transactional 注解可以声明在类上，也可以声明在方法上。在大多数情况下，方法上的事务会首先执行 
@Service
@Transactional(readOnly = true)    
public class HelloService{  
    public Foo getFoo(String fooName) {  
    }  
    //@Transactional 注解的事务设置将优先于类级别注解的事务设置   propagation:可选的传播性设置 
    @Transactional(readOnly = false, propagation = Propagation.REQUIRES_NEW)      
    public void updateFoo(Hel hel) {
    }
}  
```

## 6.整合c3p0数据库连接池

### 6.1导入依赖

```xml
	<dependency>
			<groupId>com.mchange</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.5.5</version>
		</dependency>
```

### 6.2配置applicationContext文件

```xml
<?xm1 version="1.o" encoding="UTF-8"?>
<c3p0-config>
	<defau1t-config>
		<property name="jdbcurl">jdbc:mysql://localhost:3306/xsh</property>
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="user">root</property>
		<property name="password">root</property>
		<property name="acquireIncrement">3</property>
		<property name="initialPoolSize">10</property>
		<property name="minPoolSize">2</property>
		<property name="maxPoolSize">10</property>
	</defau1t-config>
	<named-config name="oracle-config">
		<property name="jdbcurl">jdbc:mysql://localhost:3306/xsh</property>
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="user">root</property>
		<property name="password">root</property>
		<property name="acquireIncrement">3</property>
		<property name="initialPoolSize">10</property>
		<property name="minPoolSize">2</property>
		<property name="maxPoolSize">10</property>
	</named-config>
</c3p0-config>
```

### 6.3java文件中测试

```java
public class Test {
	
	public static void main(String[] args) throws PropertyVetoException, SQLException {	
	// 获得数据库连接池对象
	ComboPooledDataSource datasource=new ComboPooledDataSource("oracle-config");
	
	// 配置四大参数
	datasource.setDriverClass("com.mysql.jdbc.Driver");
	datasource.setJdbcUrl("jdbc:mysql://localhost:3306/xsh");
	datasource.setUser("root");
	datasource.setPassword("root");
	
	//配置基本池参数
	datasource.setAcquireIncrement(5);// 连接用完了自动增加几个链接
	datasource.setInitialPoolSize(20);// 最初有多少连接
	datasource.setMaxPoolSize(50);//  最大连接数量
	datasource.setMinPoolSize(2);//  最小连接数量
	
	Connection con=(Connection) datasource.getConnection();
	System.out.println(con);
	}

}
```

