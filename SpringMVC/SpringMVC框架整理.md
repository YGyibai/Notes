## 1.概述

### 1.1概念和核心


- 概念：SpringMVC是Spring框架中的一个分支，是基于Java实现MVC的轻量级Web框架
- 核心：Spring的web框架围绕**DispatcherServlet** [ 调度Servlet ] 设计的。

### 1.2什么是MVC

- Model：数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。
- View:负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。
- Controller（调度员）：	接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。
- 最常用的MVC：（Model）Bean +（view） Jsp +（Controller） Servlet

## 2.执行流程和原理

![image-20201104091141164](/Users/beifeng/Documents/笔记/笔记图片/image-20201104091141164.png)

1、 用户发送请求至前端控制器DispatcherServlet

2、 DispatcherServlet收到请求调用HandlerMapping处理器映射器。

3、 处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)

​		一并 返回给DispatcherServlet。

4、 DispatcherServlet通过HandlerAdapter处理器适配器调用处理器

5、 执行处理器(Controller，也叫后端控制器)。

6、 Controller执行完成返回ModelAndView

7、 HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet 

8、DispatcherServlet将ModelAndView传给ViewReslover视图解析器

9、 ViewReslover解析后返回具体View

10、 DispatcherServlet对View进行渲染视图(即将模型数据填充至视图中)。 

11、DispatcherServlet响应用户

### 2.2组件说明

**2.2.1 DispatcherServlet:前端控制器**

用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由 它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。

**2.2.2 HandlerMapping:处理器映射器**

HandlerMapping负责根据用户请求url找到Handler即处理器，springmvc提供了不同的映射器实现不 同的映射方式，例如:配置文件方式，实现接口方式，注解方式等。

**2.2.3 Handler:处理器**
 Handler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具

体的用户请求进行处理。 由于Handler涉及到具体的用户业务请求，所以一般情况需要程序员根据业务需求开发Handler。

**2.2.4 HandlAdapter:处理器适配器** 

通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

> **适配器模式(****Adapter Pattern****)，把一个类的接口变换成客户端所期待的另一种接口，** **Adapter****模式使原本因接口不匹配(或者不兼容)而无法在一起工作的两个类能够在一起工作。**

**2.2.5 ViewResolver:视图解析器**

 View Resolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。

 **2.2.6 View:视图**

springmvc框架提供了很多的View视图类型的支持，包括:jstlView、freemarkerView、pdfView等。 我们最常用的视图就是jsp。

**<font color="red">处理器映射器(HandlerMapping)、处理器适配器(HandlerAdapter)、视图解析器称(ViewResolver)为SpringMVC的三大核心组件</font>**





## 3.第一个SpringMVC程序

### 3.1不使用注解开发

 **继承controller接口，注册bean到容器。相当于原始servlet的写法，代码过于冗余，不够灵活，浪费时间。一般使用注解式开发**

#### 3.1.1配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--配置DispatcherServlet：SpringMVC核心-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
      	 <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个SpringMVC的resources配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:SpringMVC.xml</param-value>
        </init-param>
        <!-- 启动级别-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--拦截规则-->
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

<font color="red">**注意：拦截规则**</font>

url参与匹配的是哪部分?

答:匹配的时候并不是用完整的url进行匹配，而是用完整的url减去当前应用对应上下文之后的部分来 进行匹配。比如，访问localhost:8080/webdemo/login，应用上下文是localhost:8080/webdemo，参 与匹配的是/login

**有四种匹配机制**

- 精确匹配

  <url - pattern>中配置的项与url相应部分完全一致才能匹配上

  ```xml
  <servlet-mapping>
          <servlet-name>SpringMVC</servlet-name>
          <url-pattern>/user/user.html</url-pattern>
          <url-pattern>/index.html</url-pattern>
    			<url-pattern>/user.action</url-pattern>
    </servlet-mapping>
  ```

  在浏览器输入如下rul都会匹配到该servlet

  ```html
  localhost:8080/Demo/user/usr.html
  localhost:8080/Demo/index.html
  localhost:8080/Demo/user.action
  ```

- 路径匹配  /*

  全部拦截，包括jsp、图片、css等，建议不用 

  请求可以走到action中，但转到jsp会被再次拦截，不能访问到jsp

- 扩展名匹配 *.xx 

  拦截特定结尾的请求

- 缺省匹配 /

   拦截所有，但不包括jsp和css等静态文件，即对静态资源放行

**匹配优先级**
精确匹配 > 路径匹配 > 扩展名匹配 > 缺省匹配

 **路径匹配和扩展名匹配不能同时使用**

####   3.1.2配置springMVC.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
    <!--视图解析器ViewResolver：
		将后端处理好的数据和视图传给DispatchServlet，DS再交给ViewResolver先解析一遍，确认无误再传给前端
      -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/" />
        <property name="suffix" value=".jsp" />
    </bean>
    <!--绑定跳转的url=页面访问的网址-->
    <bean id="/hello" class="com.ssl.controller.HelloController"/>
</beans>
```

####   3.1.3HelloController实现Controller类

```java
public class HelloController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "Hello SpringMvc");
        mv.setViewName("hello");
        return mv;
    }
}
```

#### 3.1.4在WEB-INF下创建hello.jsp文件

直接访问 ：

> ​		<localhost:8080/项目名/hello>

### 3.2注解式开发

#### 3.2.1配置web.xml

​		**和2.1.1配置web.xml 代码一样，参照2.1.1web.xml**

#### 3.2.2配置springMVC.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
        xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="com.ssl.controller"/>

    <mvc:default-servlet-handler/>

    <mvc:annotation-driven/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```

#### 3.2.3创建HelloController

- 简化了实现的接口，使用@注解配置映射器

  ```java
  @Controller
  public class HelloController {
      
      @RequestMapping("/hello")
      public String hello(Model model) {
          model.addAttribute("msg", "Hello SpringMvc_annotation");
          return "hello";
      }
  ```

#### 3.2.4创建hello.jsp 进行访问

<localhost:8080/项目名/hello>

## 4.关于四个重要标签的作用和区别

### < context:annotation-config/>		

```xml
< context:annontation-config/>
```

Annontation-config 标签隐式地自动向Spring容器注册4个BeanPostProcessor(后置处理器)：

```
AutowiredAnnotationBeanPostProcessor
CommonAnnotationBeanPostProcessor
PersistenceAnnotationBeanPostProcessor
RequiredAnnotationBeanPostProcessor 
```

样就可以使用@ Resource 、@ PostConstruct、@ PreDestroy、@PersistenceContext、@Autowired、@Required等注解了，就可以实现自动注入
注册这4个 BeanPostProcessor的作用，就是为了你的系统能够识别相应的注解。

###  < context:component-scan/>

```xml
<context:component-scan base-package="com.qn.controller"/>
```

pring给我们提供了context:annotation-config 的简化的配置方式，自动帮助你完成声明，并且还自动搜索@Component , @Controller , @Service , @Repository等标注的类。

context:component-scan 除了具有context:annotation-config的功能之外，context:component-scan还可以在指定的package下扫描以及注册javabean 。还具有自动将带有@component,@service,@Repository等注解的对象注册到spring容器中的功能。

因此当使用 context:component-scan 后，就可以将 context:annotation-config移除。

### < mvc:annotation-driven/> 和< mvc:default-servlet-handler/>的作用

```xml
<!-- 
    配置defaultServletHandler的作用:
    若RequestMapping()找不到URL对应的映射，则默认的servlet(不是dispatcherServlet)会去找目标资源 
--> 
<mvc:default-servlet-handler/> 
 
<!-- 如果不加<mvc:annotation-driven />,那RequestMapping()就失效了 --> 
<mvc:annotation-driven></mvc:annotation-driven>
```

SringMVC中web.xml的dispatcherServlet的  < url-pattern>：

```xml
<servlet-mapping>
    <servlet-name>test-dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

**出现的问题：**

  " / " 将会替换掉容器的 ***\*default servlet\** ,** spring 将会拦截所有的访问请求.

   把所有的请求都交由DispatcherServlet处理.将不会再访问容器中原始默认的servlet

   但你**对静态资源的访问就是通过容器的默认servlet处理的**！故而静态资源将不可访问！

**解决的办法：**

在这种情况下，如果想要访问静态资源，通常会使用默认handler：

```java
<mvc:default-servlet-handler/>
```

 配置的这个handler通过转发所有请求到servlet容器的默认servlet来处理静态资源

使用上述配置，你会发现正常的Controller跳转失效了

 所以，使用标签：

```java
<mvc:annotation-driven/>
```

当两种标签都没有的时候，框架默认注册的有**AnnotationMethodHandlerAdapter**这个bean，所以能够处理@RequestMapping这个注解，

但是只配置了mvc:default-servlet-handler/时所注册的两个bean(**HttpRequestHandlerAdapter、SimpleControllerHandlerAdapter**)都不能处理@RequestMapping注解，因此无法找到相应的Controller，进而无法进行访问路径的映射。

当两种标签都有的时候，mvc:annotation-driven/会注册一个RequestMappingHandlerAdapter的bean，这个bean能够处理@RequestMapping这个注解。

**其中 AnnotationMethodHandlerAdapter是过期类，Spring3.2之后被RequestMappingHandlerAdapter替代。**

## **5.转发和重定向**

转发:客户浏览器发送 http 请求，Web 服务器接受此请求，调用内部的一个方法在容器内部完成 请求处理和转发动作，将目标资源发送给客户;在这里转发的路径必须是同一个 Web 容器下的 URL，其不能转向到其他的 Web 路径上，中间传递的是自己的容器内的 request。

**注意:看到的是一次的路径;request可以带过去**



重定向:客户浏览器发送 http 请求，Web 服务器接受后发送 302 状态码响应及对应新的 location 给客户浏览器，客户浏览器发现是 302 响应，则自动再发送一个新的 http 请求，请求 URL 是新 的 location 地址，服务器根据此请求寻找资源并发送给客户。

**注意:浏览器看到两次地址，request带不过去**



### 请求方法中可以出现的参数类型


javax.servlet.ServletRequest或javax.servlet.http.HttpServletRequest javax.servlet.ServletResponse或java.servlet.http.HttpServletResponse javax.servlet.http.HttpSession org.springframework.web.context.request.WebRequest或

org.springframework.web.context.request.NativeWebRequest java.util.Locale
 java.io.InputStream或java.io.Reader java.io.OutputStream或java.io.Writer

java.security.Principal
 HttpEntity<?>
 java.util.Map
 org.springframework.ui.Model
 org.springframework.ui.ModelMap org.springframework.web.servlet.mvc.support.RedirectAttributes org.springframework.validation.Errors
 org.springframework.validation.BindingResult org.springframework.web.bind.support.SessionStatus org.springframework.web.util.UriComponentsBuilder

 @PathVariable、@@MatrixVariable注解 @RequestParam、@RequestHeader、@RequestBody、@RequestPart注解 如果需要在方法中用到以上对象，可以将其写在参数列表中，spring会创建对象并传递给方法

### SpringMVC请求处理方法可返回的类型

org.springframework.web.portlet.ModelAndView org.springframework.ui.Model java.util.Map<k,v> org.springframework.web.servlet.View java.lang.String

HttpEntity或ResponseEntity
 java.util.concurrent.Callable org.springframework.web.context.request.async.DeferredResult void

### 返回值类型

1. ModelAndView
2. String
3. void
4. Object

## 6.上传与下载

### 1.导入依赖

```xml
 <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.3</version>
        </dependency>
```

### 2.前端页面

**method=post，enctype="multipart/form-data"**

```html
 <form action="test" enctype="multipart/form-data" method="post" >
        <input type="file" name="file">
        <input type="file" name="file">
        <input type="file" name="file">
        <input type="submit" value="提交" />
    </form>
```

### 3.SpringMVC.xml配置上传下载解析器

```xml
<!--   配置上传和下载解析器 -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="500000"/>
        <property name="defaultEncoding" value="UTF-8"/>
    </bean>
```

### 4.参数使用 MultipartFile接受 多个文件用MultipartFile[] 接受

```java
 @RequestMapping("test")
    public String test(MultipartFile[] file) throws IOException {
        String path="/Users/beifeng/Downloads/aa";
        for (MultipartFile f : file) {
            String newName= UUID.randomUUID()+f.getOriginalFilename();
            File pathFile= new File(path);
            if (!pathFile.exists()){
                pathFile.mkdirs();
            }
            File target=new File(path,newName);
            f.transferTo(target);
        }

        return "hello";
    }
```

## 7.拦截器

### 概念

数据独立性：Servlet中的是过滤器，而拦截器是SpringMVC框架独有的，独享request和response

拦截器只会拦截访问的控制器方法，如果访问的是jsp/html/css等式不会拦截的

拦截器是基于AOP思想的，和AOP实现是一样的，在springMVC中配置

### 自定义拦截器

```java
public class MyInterceptor implements HandlerInterceptor {

    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //return true：执行下一个拦截器
        System.out.println("===========处理前,这里进行拦截处理=================");
        return true;
    }

    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("===========处理后，通常进行日志管理=================");
    }

    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("===========清洁中=================");
    }

}
```

### 在springMVC.xml 中配置

```xml
 <!--    拦截器配置-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--            /**代表拦截所有请求-->
            <mvc:mapping path="/**"/>
            <!--            不拦截那些东西-->
            <mvc:exclude-mapping path="/index"/>
            <bean class="Interceptor.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

