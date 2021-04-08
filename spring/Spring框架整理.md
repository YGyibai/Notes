##  Spring的概念

Spring是**一个轻量级Java开发框架**，最早有**Rod Johnson**创建，目的是为了解决企业级应用开发的业务逻辑层和其他各层的耦合问题。是一个分层的JavaSE/JavaEE full-stack（一站式）轻量级开源框架，为开发Java应用程序提供全面的基础架构支持。Spring负责基础架构，因此Java开发者可以专注于应用程序的开发。

Spring最根本的使命是**解决企业级应用开发的复杂性，即简化java开发**

### 为了降低Java开发的复杂性，Spring采取了以下4种关键策略

- 基于POJO的轻量级和最小侵入性编程；

- 通过依赖注入和面向接口实现松耦合；

- 基于切面和惯例进行声明式编程；

- 通过切面和模板减少样板式代码

### spring的优缺点

#### 优点：

**1）方便解耦，简化开发**

Spring 就是一个大工厂，可以将所有对象的创建和依赖关系的维护交给 Spring 管理。

**2）方便集成各种优秀框架**

Spring 不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如 Struts2、Hibernate、MyBatis 等）的直接支持。

**3）降低 Java EE API 的使用难度**

Spring 对 Java EE 开发中非常难用的一些 API（JDBC、JavaMail、远程调用等）都提供了封装，使这些 API 应用的难度大大降低。

**4）方便程序的测试**

Spring 支持 JUnit4，可以通过注解方便地测试 Spring 程序。

**5）AOP 编程的支持**

Spring 提供面向切面编程，可以方便地实现对程序进行权限拦截和运行监控等功能。

**6）声明式事务的支持**

只需要通过配置就可以完成对事务的管理，而无须手动编程。

#### 缺点：

- Spring明明一个很轻量级的框架，却给人感觉大而全
- Spring依赖反射，反射影响性能
- 使用门槛升高，入门Spring需要较长时间

## Spring框架的主要模块

Spring框架采用分层架构，根据不同的功能划分成了多个模块，这些模块大体可分为 Data Access/Integration、Web、AOP、Aspects、Messaging、Instrumentation、Core Container 和 Test。

<img src="http://c.biancheng.net/uploads/allimg/190606/5-1Z606104H1294.gif" alt="50" style="zoom:70%;" />

#### 1. Data Access/Integration（数据访问／集成）

数据访问/集成层包括 JDBC、ORM、OXM、JMS 和 Transactions 模块，具体介绍如下。

- JDBC 模块：提供了一个 JDBC 的抽象层，大幅度减少了在开发过程中对数据库操作的编码。
- ORM 模块：对流行的对象关系映射 API，包括 JPA、JDO、Hibernate 和 iBatis 提供了的集成层。
- OXM 模块：提供了一个支持对象/XML 映射的抽象层实现，如 JAXB、Castor、XMLBeans、JiBX 和 XStream。
- JMS 模块：指 [Java]() 消息服务，包含的功能为生产和消费的信息。
- Transactions 事务模块：支持编程和声明式事务管理实现特殊接口类，并为所有的 POJO。

#### 2. Web 模块

Spring 的 Web 层包括 Web、[Servlet]()、Struts 和 Portlet 组件，具体介绍如下。

- Web 模块：提供了基本的 Web 开发集成特性，例如多文件上传功能、使用的 Servlet 监听器的 IoC 容器初始化以及 Web 应用上下文。
- Servlet模块：包括 Spring 模型—视图—控制器（MVC）实现 Web 应用程序。
- Struts 模块：包含支持类内的 Spring 应用程序，集成了经典的 Struts Web 层。
- Portlet 模块：提供了在 Portlet 环境中使用 MV C实现，类似 Web-Servlet 模块的功能。

#### 3. Core Container（核心容器）

Spring 的核心容器是其他模块建立的基础，由 Beans 模块、Core 核心模块、Context 上下文模块和 Expression Language 表达式语言模块组成，具体介绍如下。

- Beans 模块：提供了 BeanFactory，是工厂模式的经典实现，Spring 将管理对象称为 Bean。
- Core 核心模块：提供了 Spring 框架的基本组成部分，包括 IoC 和 DI 功能。
- Context 上下文模块：建立在核心和 Beans 模块的基础之上，它是访问定义和配置任何对象的媒介。ApplicationContext 接口是上下文模块的焦点。
- Expression Language 模块：是运行时查询和操作对象图的强大的表达式语言。

#### 4. 其他模块

Spring的其他模块还有 AOP、 、  以及 Test 模块，具体介绍如下。

- AOP 模块：提供了面向切面编程实现，允许定义方法拦截器和切入点，将代码按照功能进行分离，以降低耦合性。
- Aspects 模块：提供与 AspectJ 的集成，是一个功能强大且成熟的面向切面编程（AOP）框架。
-  Instrumentation 模块：提供了类工具的支持和类加载器的实现，可以在特定的应用服务器中使用。
- Test 模块：支持 Spring 组件，使用 JUnit 或 TestNG 框架的测试。

## 项目搭建

#### 1.创建maven项目

#### 2.添加依赖pom.xml文件

```xml
 <dependencies>
   <!-- 
			四个基础jar包
			spring-core
			spring-beans
			spring-context
			spring-expression
			和commons-logging 即可
	-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-expression</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.8.1</version>
        </dependency>
    </dependencies>
```

#### 3.创建applicationContext.xml文件

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">  
    <bean id="user" class="com.demo.pojo.User" />
</beans>

```

#### 4.创建pojo（user类）

#### 5.编写测试类

```java
public class FirstTest {
    @Test
    public void testl() {
        // 定义Spring配置文件的路径
        String xmlPath = "applicationContext.xml";
        // 初始化Spring容器，加载配置文件
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(   xmlPath);
        // 通过容器获取personDao实例
        User user = (User) applicationContext.getBean("user");
      	System.out.println(user);
    }
}
```

## IOC和DI  （面试高频）

**控制反转，依赖注入**

IOC控制反转：说的是创建对象实例的控制权从代码控制剥离到IOC容器控制，实际就是你在xml文件控制，侧重于原理。

DI依赖注入：说的是创建对象实例时，为这个对象注入属性值或其它对象实例，侧重于实现。

IoC和DI其实是同一个概念的不同角度描述，DI相对IoC而言，明确描述了被管理的对象中，依赖的属性也应该由Spring容器自动注入

### IoC容器

Spring 提供了两种 IoC 容器，分别为 BeanFactory 和 ApplicationContext，接下来将针对这两种 IoC 容器进行详细讲解。

#### BeanFactory

BeanFactory 是基础类型的 IoC 容器，它由 org.springframework.beans.facytory.BeanFactory 接口定义，并提供了完整的 IoC 服务支持。简单来说，BeanFactory 就是一个管理 Bean 的工厂，它主要负责初始化各种 Bean，并调用它们的生命周期方法。

BeanFactory 接口有多个实现类，最常见的是 org.springframework.beans.factory.xml.XmlBeanFactory，它是根据 XML 配置文件中的定义装配 Bean 的。

创建 BeanFactory 实例时，需要提供 Spring 所管理容器的详细配置信息，这些信息通常采用 XML 文件形式管理。其加载配置信息的代码具体如下所示：

```java
BeanFactory beanFactory = new XmlBeanFactory(new FileSystemResource("D://applicationContext.xml"));
```



#### ApplicationContext

<font color="Magenta">ApplicationContext 是 BeanFactory 的子接口，也被称为应用上下文</font>。

该接口的全路径为 org.springframework.context.ApplicationContext，它不仅提供了 BeanFactory 的所有功能，还添加了对 i18n（国际化）、资源访问、事件传播等方面的良好支持。

ApplicationContext 接口有两个常用的实现类，具体如下。

##### 1）ClassPathXmlApplicationContext

该类从类路径 ClassPath 中寻找指定的 XML 配置文件，找到并装载完成 ApplicationContext 的实例化工作，具体如下所示。

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext(String configLocation);
```

在上述代码中，configLocation 参数用于指定 Spring 配置文件的名称和位置，如 applicationContext.xml。

##### 2）FileSystemXmlApplicationContext

该类从指定的文件系统路径中寻找指定的 XML 配置文件，找到并装载完成 ApplicationContext 的实例化工作，具体如下所示。

```java
ApplicationContext applicationContext = new FileSystemXmlApplicationContext(String configLocation);
```

**它与 ClassPathXmlApplicationContext 的区别是：**

在读取 Spring 的配置文件时，FileSystemXmlApplicationContext 不再从类路径中读取配置文件，而是通过参数指定配置文件的位置，它可以获取类路径之外的资源，如“F：/workspaces/applicationContext.xml”。

<font color="Magenta">**BeanFactory 和 ApplicationContext 都是通过 XML 配置文件加载 Bean 的。**</font>

二者的主要区别在于，如果 Bean 的某一个属性没有注入，则使用 BeanFacotry 加载后，在第一次调用 getBean() 方法时会抛出异常，而 ApplicationContext 则在初始化时自检，这样有利于检查所依赖的属性是否注入。

因此，在实际开发中，通常都选择使用 ApplicationContext，而只有在系统资源较少时，才考虑使用 BeanFactory。本教程中使用的就是 ApplicationContext。

#### 创建对象（bean）的三种方式

##### 参构造

 XML配置：

```html
<!-- 默认的无参构建 -->

    <bean id="user" class="ioc.pojo.User"></bean>
```

 测试：

```java
@Test
public void testUser(){

    //  过得容器
    
	ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");

	// 默认的无参构造创建

	User user = (User) context.getBean("user");

	System.out.println("默认的无参构造创建:" + user);
}
```



##### 静态工厂

 配置工厂类：

```java

public class UserFactory {
	// 静态方法
	public static User getUser1() {
		return new User();
	}
}
```

 XML配置：

```html
  <!-- 使用静态工厂创建user -->
  <bean id="user1" class="ioc.service.UserFactory" factory-method="getUser1"></bean>
```

 class 指的是该工厂类的包路径，factory-method 指的是该工厂类创建Bean的静态方法。注意：这里一定要静态方法

 测试：

```java
@Test
public void testUser1(){

	ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");

		// 静态工厂创建

	User user1 = (User) context.getBean("user1");

	System.out.println("静态工厂创建:" + user1);

}
```



##### 实例工厂

 配置工厂类：

```java

public class UserFactory {
	//普通方法
	public User getUser2() {
		return new User();
	}
}
```

XML配置：

```html
<!-- 使用实例工厂创建 user -->
    <bean id="userFactory" class="ioc.service.UserFactory"></bean>
    <bean id="user2" factory-bean="userFactory" factory-method="getUser2"></bean>
```

测试：

```java
	@Test
	public void testUser2(){
		ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		// 实例工厂创建
		User user2 = (User) context.getBean("user2");
		System.out.println("实例工厂创建:" + user2);
	}
```

##### 三者的区别

这三种方式最根本的区别还是创建方式的不同。

第一种，通过默认的无参构造方式创建，其本质就是把类交给Spring自带的工厂（BeanFactory）管理、由Spring自带的工厂模式帮我们维护和创建这个类。如果是有参的构造方法，也可以通过XML配置传入相应的初始化参数，这种也是开发中用的最多的。

第二种，通过静态工厂创建，其本质就是把类交给我们自己的静态工厂管理，Spring只是帮我们调用了静态工厂创建实例的方法，而创建实例的这个过程是由我们自己的静态工厂实现的，在实际开发的过程中，很多时候我们需要使用到第三方jar包提供给我们的类,而这个类没有构造方法，而是通过第三方包提供的静态工厂创建的，这是时候，如果我们想把第三方jar里面的这个类交由spring来管理的话，就可以使用Spring提供的静态工厂创建实例的配置。

第三种，通过实例工厂创建，其本质就是把创建实例的工厂类交由Spring管理，同时把调用工厂类的方法创建实例的这个过程也交由Spring管理，看创建实例的这个过程也是有我们自己配置的实例工厂内部实现的。在实际开发的过程中，如Spring整合Hibernate就是通过这种方式实现的。但对于没有与Spring整合过的工厂类，我们一般都是自己用代码来管理的。

## 依赖注入

### 4.1   Set方法（重点）

- **基本类型**

  ```xml
  <bean id ="xxx"  class="com.xxx.pojo.xxx">   <!--id: 对应的getbean方法applicationContext.getBean("xxx");-->
  <property name="name" value="值"></property>  <!-- name=" xxx ":映射文件和实体类对应的名字一一相同 -->
  //数组
  <property>
  <array>
  <value>值</value>
  </array>
  </property>
  ```

- **集合、属性**

  - list

    ```xml
    <list>
    <value>值</value>
    </list>
    ```

  - set

    同下

  - map

    ```xml
    "<entry key="键" value="xxx值"></entry>"
    ```

    ref : 引用或者依赖

  - properties

     

    ```xml
    <property>
    	<props>
    		<prop key="键" >xxx值</prop>
    	</props>
    </property>
    ```

    

  - null

- 引用类型

  ```xml
  <bean id ="car"  class="com.sptest.pojo.car">   <!--id: 对应的getbean方法applicationContext.getBean("user");-->
  <property name="name" value="值"></property>  <!-- name=" xxx ":映射文件和实体类对应的名字一一相同 -->
  
  <bean>
  <property name="car" ref="car"></property>
  </bean>
  
  ```

  **ref  引用类型**
   **<value.../>主要是配置基本类型的属性值，但是如果我们需要为Bean设置属性值是另一个Bean实例时，这个时候需要使用<ref.../>元素。使用<ref.../>元素可以指定如下两个属性。**
   **bean:引用不在同一份XML配置文件中的其他Bean实例的id属性值。**

   **local：引用同一份XML配置文件中的其他Bean实例的id属性值。**

  其实Spring提供了一种更加简洁的写法：

  ```
  <bean id="steelAxe" class="com.spring.service.impl.SteelAce"></bean>  
  <bean id="chinese" class="com.spring.service.impl.Chinese" >  
      <property name="axe" ref="steelAxe" />  
  </bean>  
  ```

     **通过property增加ref属性，一样可以将另一个Bean的引用设置成axe属性值。这样写的效果和使用<ref.../>属性一样，而且不需要区分是使用bean属性还是local属性，所以推荐这种写法。**

### **4.2  构造方法**

- name
- value
- index
- type

```xml
要注意<bean>标签里class默认走的是无参构造
<bean id="xxx"  class="com.xxx.pojo.xxx">
	<constructor-arg name="对应的谁" value="值">
	</constructor-arg>
	
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
	 </bean>

 arg  参数的意思
```





### 4.3  P命名空间（原理为set）

```java
xmlns:p="http://www.springframework.org/schema/p"
```



### 4.4  C命名空间(原理为构造)

```
xmlns:c="http://www.springframework.org/schema/c"
```



### 4.5  sepl（spring expression language）spring表达语言





##  bean的作用域和模块化

### scope属性

Spring 容器在初始化一个 Bean 的实例时，同时会指定该实例的作用域。Spring3 为 Bean 定义了五种作用域，具体如下。

#### 1）singleton

单例模式，使用 singleton 定义的 Bean 在 Spring 容器中只有一个实例，这也是 Bean **默认**的作用域。 	

#### 2）prototype

原型模式，每次通过 Spring 容器获取 prototype 定义的 Bean 时，容器都将创建一个新的 Bean 实例。

#### 3）request

在一次 HTTP 请求中，容器会返回该 Bean 的同一个实例。而对不同的 HTTP 请求，会返回不同的实例，该作用域仅在当前 HTTP Request 内有效。

#### 4）session

在一次 HTTP Session 中，容器会返回该 Bean 的同一个实例。而对不同的 HTTP 请求，会返回不同的实例，该作用域仅在当前 HTTP Session 内有效。

#### 5）global Session

在一个全局的 HTTP Session 中，容器会返回该 Bean 的同一个实例。该作用域仅在使用 portlet context 时有效。

##### singleton 作用域

singleton 是 Spring 容器默认的作用域，当一个 Bean 的作用域为 singleton 时，Spring 容器中只会存在一个共享的 Bean 实例，并且所有对 Bean 的请求，只要 id 与该 Bean 定义相匹配，就只会返回 Bean 的同一个实例。

##### prototype 作用域

使用 prototype 作用域的 Bean 会在每次请求该 Bean 时都会创建一个新的 Bean 实例。因此对需要保持会话状态的 Bean（如 [Struts2](http://c.biancheng.net/struts2/) 的 Action 类）应该使用 prototype 作用域。



### 5.2   生命周期

[Spring](http://c.biancheng.net/spring/) 容器可以管理 singleton 作用域 Bean 的生命周期，在此作用域下，Spring 能够精确地知道该 Bean 何时被创建，何时初始化完成，以及何时被销毁。

而对于 prototype 作用域的 Bean，Spring 只负责创建，当容器创建了 Bean 的实例后，Bean 的实例就交给客户端代码管理，Spring 容器将不再跟踪其生命周期。每次客户端请求 prototype 作用域的 Bean 时，Spring 容器都会创建一个新的实例，并且不会管那些被配置成 prototype 作用域的 Bean 的生命周期。

了解 Spring 生命周期的意义就在于，可以利用 Bean 在其存活期间的指定时刻完成一些相关操作。这种时刻可能有很多，但一般情况下，会在 Bean 被初始化后和被销毁前执行一些相关操作。

在 Spring 中，Bean 的生命周期是一个很复杂的执行过程，我们可以利用 Spring 提供的方法定制 Bean 的创建过程。

当一个 Bean 被加载到 Spring 容器时，它就具有了生命，而 Spring 容器在保证一个 Bean 能够使用之前，会进行很多工作。Spring 容器中 Bean 的生命周期流程如图 1 所示。



![Bean的生命周期](http://c.biancheng.net/uploads/allimg/190701/5-1ZF1100325116.png)
图 1 Bean 的生命周期

##### **Bean 生命周期的整个执行过程描述如下。（需要背）**

**1）根据配置情况调用 Bean 构造方法或工厂方法实例化 Bean。**

**2）利用依赖注入完成 Bean 中所有属性值的配置注入。**

**3）如果 Bean 实现了 BeanNameAware 接口，则 Spring 调用 Bean 的 setBeanName() 方法传入当前 Bean 的 id 值。**

**4）如果 Bean 实现了 BeanFactoryAware 接口，则 Spring 调用 setBeanFactory() 方法传入当前工厂实例的引用。**

**5）如果 Bean 实现了 ApplicationContextAware 接口，则 Spring 调用 setApplicationContext() 方法传入当前 ApplicationContext 实例的引用。**

**6）如果 BeanPostProcessor 和 Bean 关联，则 Spring 将调用该接口的预初始化方法 postProcessBeforeInitialzation() 对 Bean 进行加工操作，此处非常重要，Spring 的 AOP 就是利用它实现的。**

**7）如果 Bean 实现了 InitializingBean 接口，则 Spring 将调用 afterPropertiesSet() 方法。**

**8）如果在配置文件中通过 init-method 属性指定了初始化方法，则调用该初始化方法。**

**9）如果 BeanPostProcessor 和 Bean 关联，则 Spring 将调用该接口的初始化方法 postProcessAfterInitialization()。此时，Bean 已经可以被应用系统使用了。**

**10）如果在 <bean> 中指定了该 Bean 的作用范围为 scope="singleton"，则将该 Bean 放入 Spring IoC 的缓存池中，将触发 Spring 对该 Bean 的生命周期管理；如果在 <bean> 中指定了该 Bean 的作用范围为 scope="prototype"，则将该 Bean 交给调用者，调用者管理该 Bean 的生命周期，Spring 不再管理该 Bean。**

**11）如果 Bean 实现了 DisposableBean 接口，则 Spring 会调用 destory() 方法将 Spring 中的 Bean 销毁；如果在配置文件中通过 destory-method 属性指定了 Bean 的销毁方法，则 Spring 将调用该方法对 Bean 进行销毁。**

**Spring 为 Bean 提供了细致全面的生命周期过程，通过实现特定的接口或 <bean> 的属性设置，都可以对 Bean 的生命周期过程产生影响。虽然可以随意配置 <bean> 的属性，但是建议不要过多地使用 Bean 实现接口，因为这样会导致代码和 Spring 的聚合过于紧密。**

### 5.3模块化

include

## 自动装配和注解开发

#### 自动装配

**spring会在上下文中自动寻找，自动给Bean装配属性**

**分为三种方式**

#####  在xml中显示配置

#####  隐式的自动装配（重要）

###### **autowire属性自动装配**

Spring支持自动装配Bean与Bean之间的依赖关系，也就是说我们无需显示的指定依赖Bean。由BeanFactory检查XML配置文件内容，根据某种规则，为主调Bean注入依赖关系。

Spring的自动装配机制可以通过<bean.../>元素的default-autowire属性指定，也可以通过<bean.../>元素的**autowire**属性指定。

​     自动装配可以减少配置文件的工作量，但是它降低了依赖关系的透明性和清晰性，所以一般来说在较大部署环境中不推荐使用，显示配置合作者能够得到更加清晰的依赖关系。Spring提供了如下几种规则来实现自动装配。

| 名称        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| byName      | 根据 Property 的 name 自动装配，如果一个 Bean 的 name 和另一个 Bean 中的 Property 的 name 相同，则自动装配这个 Bean 到 Property 中。 |
| byType      | 根据 Property 的数据类型（Type）自动装配，如果一个 Bean 的数据类型兼容另一个 Bean 中 Property 的数据类型，则自动装配。 |
| constructor | 根据构造方法的参数的数据类型，进行 byType 模式的自动装配。   |
| autodetect  | 如果发现默认的构造方法，则用 constructor 模式，否则用 byType 模式。 |
| no          | 默认情况下，不使用自动装配，Bean 依赖必须通过 ref 元素定义。 |

###### 注解自动装配

**导入注解**

```dtd
xmlns : context = "http://www.springframework.org/schema/context "
xsi:schemaLocation="http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.2.xsd" 
```

**配置注解支持**

```
<context : annotation - config/>    开启注解	
```

在 [Spring](http://c.biancheng.net/spring/) 中，尽管使用 XML 配置文件可以实现 Bean 的装配工作，但如果应用中 Bean 的数量较多，会导致 XML 配置文件过于臃肿，从而给维护和升级带来一定的困难。

[Java](http://c.biancheng.net/java/) 从 JDK 5.0 以后，提供了 Annotation（注解）功能，Spring 也提供了对 Annotation 技术的全面支持。Spring3 中定义了一系列的 Annotation（注解），常用的注解如下。

**1）@Component**

可以使用此注解描述 Spring 中的 Bean，但它是一个泛化的概念，仅仅表示一个组件（Bean），并且可以作用在任何层次。使用时只需将该注解标注在相应类上即可。

**2）@Repository**

用于将数据访问层（DAO层）的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

**3）@Service**

通常作用在业务层（Service 层），用于将业务层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

**4）@Controller**

通常作用在控制层（如 [Struts2](http://c.biancheng.net/struts2/) 的 Action），用于将控制层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

**5）@Autowired**

用于对 Bean 的属性变量、属性的 Set 方法及构造函数进行标注，配合对应的注解处理器完成 Bean 的自动配置工作。默认按照 Bean 的类型进行装配。

**6）@Resource**

其作用与 Autowired 一样。其区别在于 @Autowired 默认按照 **Bean 类型**装配，而 @Resource 默认按照 Bean **实例名称**进行装配。

@Resource 中有两个重要属性：name 和 type。

Spring 将 name 属性解析为 Bean 实例名称，type 属性解析为 Bean 实例类型。如果指定 name 属性，则按实例名称进行装配；如果指定 type 属性，则按 Bean 类型进行装配。

如果都不指定，则先按 Bean 实例名称装配，如果不能匹配，则再按照 Bean 类型进行装配；如果都无法匹配，则抛出 NoSuchBeanDefinitionException 异常。

**type="xxx.class"   type指定的是class文件    传的是一个反射**

**7）@Qualifier**

与 @Autowired 注解配合使用，会将默认的按 Bean 类型装配修改为按 Bean 的实例名称装配，Bean 的实例名称由 @Qualifier 注解的参数指定。

#####  在Java中显示配置



#### 注解开发

spring4之后，使用注解开发，需导入aop包

jdk1.5支持注解，spring2.5支持注解

**步骤：**

- 导入aop包
- 添加context约束
- 扫描指定包

```xml
<context:component-scan base-package="xxx.pojo?"
```

**配置注解支持**

```xml
<context : annotation-config/>    开启注解	
```

**1）@Component**

组件，把该类交给spring托管，2,3,4注解为同一个意思，因为开发时 需要在实体类打注解，controll打注解，service打注解，数据库也要打注解（mapper），分为不同的包不同的功能，一眼看过去不知道是干嘛的，所以新加2.3.4三个注解

**2）@Repository**

数据库

**3）@Service**

服务层

**4）@Controller**

控制层

**5）@Autowired**

自动拆箱自动装箱

**6）@Scope**

作用域，范围（单例，原型）

**7）@PostConstruct**

初始化

**8)@PreDestroy**

销毁

**9）需要手动赋值，在字段或对应的set上加@value**

- 数组： @value=("xx,xx,xx")
- List:  同上
- set：
- map： 

#### 6.3注解开发和传统xml开发的优缺点

**注解开发**

- **优点：**
  - 保存在 class 文件中，降低维护成本
  - 无需工具支持，无需解析
  - 编译期即可验证正确性，查错变得容易
  - 提升开发效率

- **缺点：**
  -  复杂类型注入不便，不便于维护
  -  若要对配置项进行修改，不得不修改 Java 文件，重新编译打包应用
  -  配置项编码在 Java 文件中，可扩展性差

**xml开发**

- **优点：**
  - xml 作为可扩展标记语言最大的优势在于开发者能够为软件量身定制适用的标记，使代码更加通俗易懂
  - 利用 xml 配置能使软件更具扩展性。例如 Spring 将 class 间的依赖配置在 xml 中，最大限度地提升应用的可扩展性
  - 具有成熟的验证机制确保程序正确性。利用 Schema 或 DTD 可以对 xml 的正确性进行验证，避免了非法的配置导致应用程序出错
  - 修改配置而无需变动现有程序

- **缺点：**

  -  需要解析工具或类库的支持

  -  解析 xml 势必会影响应用程序性能，占用系统资源

  -  配置文件过多导致管理变得困难

  -  编译期无法对其配置项的正确性进行验证，或要查错只能在运行期

  -  IDE 无法验证配置项的正确性无能为力

  -  查错变得困难。往往配置的一个手误导致莫名其妙的错误

  -  开发人员不得不同时维护代码和配置文件，开发效率变得低下

  -  配置项与代码间存在潜规则。改变了任何一方都有可能影响另外一方

## Spring整合单元测试

添加juit依赖

添加Spring-test依赖

```xml
 				<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
         <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
```

编写测试类

```java
//通过SpringJUnit4ClassRunner运行
@RunWith(SpringJUnit4ClassRunner.class)
//加载applicationContext配置文件
@ContextConfiguration("classpath:applicationContext.xml")
public class Test1 {
  //自动装配User对象
    @Autowired
    User user;
    @Test
    public void test(){
        System.out.println(user);
    }
}
```

## 面向切面编程（AOP）

面向切面编程（AOP）和面向对象编程（OOP）类似，也是一种编程模式。[Spring](http://c.biancheng.net/spring/) AOP 是基于 AOP 编程模式的一个框架，它的使用有效减少了系统间的重复代码，达到了模块间的松耦合目的。

<font color="red">AOP 的全称是“Aspect Oriented Programming”，即面向切面编程，它将业务逻辑的各个部分进行隔离，使开发人员在编写业务逻辑时可以专心于核心业务，从而提高了开发效率。</font>

AOP 采取横向抽取机制，取代了传统纵向继承体系的重复性代码，其应用主要体现在事务处理、日志管理、权限控制、异常处理等方面。

目前最流行的 AOP 框架有两个，分别为 Spring AOP 和 AspectJ。

Spring AOP 使用纯 [Java](http://c.biancheng.net/java/) 实现，不需要专门的编译过程和类加载器，在运行期间通过代理方式向目标类植入增强的代码。

AspectJ 是一个基于 Java 语言的 AOP 框架，从 Spring 2.0 开始，Spring AOP 引入了对 AspectJ 的支持。AspectJ 扩展了 Java 语言，提供了一个专门的编译器，在编译时提供横向代码的植入。

为了更好地理解 AOP，就需要对 AOP 的相关术语有一些了解，这些专业术语主要包含 Joinpoint、Pointcut、Advice、Target、Weaving、Proxy 和 Aspect，它们的含义如下表所示。

| 名称                | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| Joinpoint（连接点） | 指那些被拦截到的点，在 Spring 中，可以被动态代理拦截目标类的方法。 |
| Pointcut（切入点）  | 指要对哪些 Joinpoint 进行拦截，即被拦截的连接点。            |
| Advice（通知）      | 指拦截到 Joinpoint 之后要做的事情，即对切入点增强的内容。    |
| Target（目标）      | 指代理的目标对象。                                           |
| Weaving（植入）     | 指把增强代码应用到目标上，生成代理对象的过程。               |
| Proxy（代理）       | 指生成的代理对象。                                           |
| Aspect（切面）      | 切入点和通知的结合。                                         |

### 动态代理的方式

- JDK 动态代理是通过 JDK 中的 java.lang.reflect.Proxy 类实现的

- CGLIB动态代理（Code Generation Library）是一个高性能开源的代码生成包，它被许多 AOP 框架所使用，其底层是通过使用一个小而快的字节码处理框架 ASM（[Java](http://c.biancheng.net/java/) 字节码操控框架）转换字节码并生成新的类。

- javasist-class

- springapi （底层还是动态代理 接口类似的开发）

- ApsectJ（重要）

  - xml

  - （annotation）注解

### ApsectJ  AOP具体实现



两种实现方式：一种是spring aop，几乎是原生开发，比较麻烦。
							一种是AspectJ开发，这是一个aop框架，开发比较常用。

#### 通知类型

- 前置通知
- 后置通知（如果出现异常不会调用）
- 环绕通知
- 异常拦截通知
- 后置通知（无论是否有异常都会调用）
 <font color="red">**AspectJ实现**</font>

导入依赖:需要aop包和aspectj，同时添加aop约束

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
</dependency>
```



#### 基于xml

##### 1.创建切面类

```java
public class MyAspect {
    // 前置通知
    public void myBefore(JoinPoint joinPoint) {
        System.out.print("前置通知，目标：");
        System.out.print(joinPoint.getTarget() + "方法名称:");
        System.out.println(joinPoint.getSignature().getName());
    }
    // 后置通知
    public void myAfterReturning(JoinPoint joinPoint) {
        System.out.print("后置通知，方法名称：" + joinPoint.getSignature().getName());
    }
    // 环绕通知
    public Object myAround(ProceedingJoinPoint proceedingJoinPoint)
            throws Throwable {
        System.out.println("环绕开始"); // 开始
        Object obj = proceedingJoinPoint.proceed(); // 执行当前目标方法
        System.out.println("环绕结束"); // 结束
        return obj;
    }
    // 异常通知
    public void myAfterThrowing(JoinPoint joinPoint, Throwable e) {
        System.out.println("异常通知" + "出错了" + e.getMessage());
    }
    // 最终通知
    public void myAfter() {
        System.out.println("最终通知");
    }
}
```

- 通过 JoinPoint 参数可以获得目标对象的类名、目标方法名和目标方法参数等。

- 需要注意的是，环绕通知必须接收一个类型为 ProceedingJoinPoint 的参数，返回值必须是 Object 类型，且必须抛出异常。

- 异常通知中可以传入 Throwable 类型的参数，用于输出异常信息。

##### 2.创建applicationContext配置文件

切入点配置：

```
    public void com.xxx.yyy.UserServiceImpl.save()
    void com.xxx.yyy.UserServiceImpl.save()
    * com.xxx.yyy.UserServiceImpl.save()
    * com.xxx.yyy.UserServiceImpl.*()
    * com.xxx.yyy.*Impl.*(..) //..表示任何参数列表都可以
    * com.xxx.yyy..*Impl.*(..)  //可以连带子包一起查询    注意*后面有空格！！！
```

重要示例

```xml
    <!--目标类 -->
    <bean id="customerDao" class="com.mengma.dao.CustomerDaoImpl" />
    <!--切面类 -->
    <bean id="myAspect" class="com.mengma.aspectj.xml.MyAspect"></bean>
    <!--AOP 编程 -->
    <aop:config>
        <aop:aspect ref="myAspect">
            <!-- 配置切入点，通知最后增强哪些方法 -->
            <aop:pointcut expression="execution ( * com.mengma.dao.*.* (..))"
                id="myPointCut" />
            <!--前置通知，关联通知 Advice和切入点PointCut -->
            <aop:before method="myBefore" pointeut-ref="myPointCut" />
            <!--后置通知，在方法返回之后执行，就可以获得返回值returning 属性 -->
            <aop:after-returning method="myAfterReturning"
                pointcut-ref="myPointCut" returning="returnVal" />
            <!--环绕通知 -->
            <aop:around method="myAround" pointcut-ref="myPointCut" />
            <!--抛出通知：用于处理程序发生异常，可以接收当前方法产生的异常 -->
            <!-- *注意：如果程序没有异常，则不会执行增强 -->
            <!-- * throwing属性：用于设置通知第二个参数的名称，类型Throwable -->
            <aop:after-throwing method="myAfterThrowing"
                pointcut-ref="myPointCut" throwing="e" />
            <!--最终通知：无论程序发生任何事情，都将执行 -->
            <aop:after method="myAfter" pointcut-ref="myPointCut" />
        </aop:aspect>
    </aop:config>
```

##### 3.创建测试类进行测试

```java
  ApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
  CustomerDao test=(CustomerDao) context.getBean("customerDao");
  test.add();
```

#### 通过（Annotation）注解的方式aop

注解的介绍如表 1 所示。

| 名称            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| @Aspect         | 用于定义一个切面。                                           |
| @Before         | 用于定义前置通知，相当于 BeforeAdvice。                      |
| @AfterReturning | 用于定义后置通知，相当于 AfterReturningAdvice。              |
| @Around         | 用于定义环绕通知，相当于MethodInterceptor。                  |
| @AfterThrowing  | 用于定义抛出通知，相当于ThrowAdvice。                        |
| @After          | 用于定义最终final通知，不管是否异常，该通知都会执行。        |
| @DeclareParents | 用于定义引介通知，相当于IntroductionInterceptor（不要求掌握）。 |
| @PointCut       | 定义切入点，用于取代：<aop:pointcut expression="execution(*com.mengma.dao..*.*(..))" id="myPointCut"/>要求：方法必须是private，没有值，名称自定义，没有参数 |

##### 1.配置applicationContext.xml

```xml
 		<!--扫描含com.mengma包下的所有注解-->
    <context:component-scan base-package="com.mengma"/>
    <!-- 使切面开启自动代理 -->
    <aop:aspectj-autoproxy/>
```

##### 2.创建切面类

```java
//切面类
@Aspect
@Component
public class MyAspect {
    // 用于取代：<aop:pointcut
    // expression="execution(*com.mengma.dao..*.*(..))" id="myPointCut"/>
    // 要求：方法必须是private，没有值，名称自定义，没有参数
    @Pointcut("execution(*com.mengma.dao..*.*(..))")
    private void myPointCut() {
    }
    // 前置通知
    @Before("myPointCut()")
    public void myBefore(JoinPoint joinPoint) {
        System.out.print("前置通知，目标：");
        System.out.print(joinPoint.getTarget() + "方法名称:");
        System.out.println(joinPoint.getSignature().getName());
    }
    // 后置通知
    @AfterReturning(value = "myPointCut()")
    public void myAfterReturning(JoinPoint joinPoint) {
        System.out.print("后置通知，方法名称：" + joinPoint.getSignature().getName());
    }
    // 环绕通知
    @Around("myPointCut()")
    public Object myAround(ProceedingJoinPoint proceedingJoinPoint)
            throws Throwable {
        System.out.println("环绕开始"); // 开始
        Object obj = proceedingJoinPoint.proceed(); // 执行当前目标方法
        System.out.println("环绕结束"); // 结束
        return obj;
    }
    // 异常通知
    @AfterThrowing(value = "myPointCut()", throwing = "e")
    public void myAfterThrowing(JoinPoint joinPoint, Throwable e) {
        System.out.println("异常通知" + "出错了" + e.getMessage());
    }
    // 最终通知
    @After("myPointCut()")
    public void myAfter() {
        System.out.println("最终通知");
    }
}
```

##### 3.创建测试类

```java
public class AnnotationTest {
    @Test
    public void test() {        
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(
                “applicationContext.xml”);
        // 从spring容器获取实例
        CustomerDao customerDao = (CustomerDao) applicationContext
                .getBean("customerDao");
        // 执行方法
        customerDao.add();
    }
}
```

