这是SpringBoot项目的启动类

```java
@MapperScan("com.gq.mapper")
@SpringBootApplication
public class GqwpApplication {
    public static void main(String[] args) {
        SpringApplication.run(GqwpApplication.class, args);
    }
}
```

​		从上述代码可以看出，核心代码只有Annotation定义（@SpringBootApplication）和类定义（SpringApplication.run)。

## 一、@SpringBootApplication

@SpringBootApplication注解是Spring Boot的核心注解，它其实是一个组合注解。

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

```

重要注释：

> This is a convenience
> annotation that is equivalent to declaring {@code @Configuration},{@code @EnableAutoConfiguration} and {@code @ComponentScan}.

译文：

​	

> 这是一个方便注释，等效于声明{@code @Configuration}，{@ code @EnableAutoConfiguration}和{@code @ComponentScan}。

从中可以看出@SpringBootApplication是由这三个Annotation组成的，接下来分别介绍这3个Annotation。

### @SpringBootConfiguration

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration 
```

​		任何一个标注了@Configuration的Java类定义都是一个JavaConfig配置类。SpringBoot社区推荐使用基于JavaConfig的配置形式，所以，这里的启动类标注了@Configuration之后，本身其实也是一个IoC容器的配置类。

### @ComponentScan

```java
@ComponentScan(excludeFilters = {
@Filter(type = FilterType.CUSTOM, classes = ypeExcludeFilter.class),
@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
```

​		@componentScan自动扫描并加载自动配置的的过滤器（以及它的过滤规则）		@ComponentScan的功能其实就是自动扫描并加载符合条件的组件（比如@Component和@Repository等）或者bean定义，最终将这些bean定义加载到IoC容器中。

​		我们可以通过basePackages等属性来细粒度的定制@ComponentScan自动扫描的范围，如果不指定，则默认Spring框架实现会从声明@ComponentScan所在类的package进行扫描。

​		注：所以SpringBoot的启动类最好是放在root package下，因为默认不指定basePackages。

### @EnableAutoConfiguration

#### **关于@import注解**

​		@import注解：通过导入的方式实现把实例加入springIOC容器中

@Import的三种使用方式

```java
public @interface Import {
    /**
     * {@link Configuration}, {@link ImportSelector}, {@link ImportBeanDefinitionRegistrar}
     * or regular component classes to import.
     */
    Class<?>[] value();
}
```

有兴趣的朋友可以去看看[@impor注解](https://www.cnblogs.com/iiiiiher/p/12781618.html)，这里就不做介绍了。

​		@EnableAutoConfiguration也是借助@Import的帮助，将所有符合自动配置条件的bean定义加载到IoC容器

​		@EnableAutoConfiguration会根据类路径中的jar依赖为项目进行自动配置，如：添加了spring-boot-starter-web依赖，会自动添加Tomcat和Spring MVC的依赖，Spring Boot会对Tomcat和Spring MVC进行自动配置

@EnableAutoConfiguration关键信息如下

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration 
```

​		@EnableAutoConfiguration注解引入了@AutoConfigurationPackage和@Import这两个注解。@AutoConfigurationPackage的作用就是自动配置的包，@Import导入需要自动配置的组件。

#### @AutoConfigurationPackage

进入@AutoConfigurationPackage，发现也是引入了@Import注解

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Import(AutoConfigurationPackages.Registrar.class)
public @interface AutoConfigurationPackage {
```

AutoConfigurationPackages.Registrar.class

```java
static class Registrar implements ImportBeanDefinitionRegistrar, DeterminableImports {
		@Override
		public void registerBeanDefinitions(AnnotationMetadata metadata,BeanDefinitionRegistry registry) {
			register(registry, new PackageImports(metadata).getPackageNames().toArray(new String[0]));
		}
		@Override
	public Set<Object> determineImports(AnnotationMetadata metadata) {
			return Collections.singleton(new PackageImports(metadata));
		}
}
```

 new PackageImports(metadata).getPackageNames().toArray(new String[0]),
new PackageImports(metadata)。
这两句代码的作用是将启动类所在包下的主类和子类中的所有组件注册到Spring容器中。

#### AutoConfigurationImportSelector.class

```java
//此方法根据配置类的注释元数据获取被导入的自动配置。
protected AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
		if (!isEnabled(annotationMetadata)) {
			return EMPTY_ENTRY;
		}
		AnnotationAttributes attributes = getAttributes(annotationMetadata);
		List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
		configurations = removeDuplicates(configurations);
		Set<String> exclusions = getExclusions(annotationMetadata, attributes);
		checkExcludedClasses(configurations, exclusions);
		configurations.removeAll(exclusions);
		configurations = getConfigurationClassFilter().filter(configurations);
		fireAutoConfigurationImportEvents(configurations, exclusions);
		return new AutoConfigurationEntry(configurations, exclusions);
	}
```

```java
//此方法使用SpringFactoriesLoader和getSpringFactoriesLoaderFactoryClass()加载候选的自动配置
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
		List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
				getBeanClassLoader());
		Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
				+ "are using a custom packaging, make sure that file is correct.");
		return configurations;
	}
```

​		SpringFactoriesLoader.loadFactoryNames方法调用loadSpringFactories方法从所有的jar包中读取META-INF/spring.factories文件信息。

​	下面是spring-boot-autoconfigure这个jar中spring.factories文件部分内容，其中有一个key为org.springframework.boot.autoconfigure.EnableAutoConfiguration的值定义了需要自动配置的bean，通过读取这个配置获取一组@Configuration类

```xml-dtd
# Auto Configuration Import Filters
org.springframework.boot.autoconfigure.AutoConfigurationImportFilter=\
org.springframework.boot.autoconfigure.condition.OnClassCondition

# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
```

​		实际上，这些xxxAutoConfiguratio不是所有都会被加载，会根据xxxAutoConfiguration上的 @see ConditionalOnBean、 @see ConditionalOnMissingBean、@see ConditionalOnClass等条件判断是否加载。

## 总结

- **@ComponentScan**

  ​		自动扫描并加载符合条件的组件

- **@SpringBootConfiguration**

  ​		将打上@SpringBootConfiguration标签的成为一个配置类，启动并加载

- **@EnableAutoConfiguration：**

  ​		**从classpath中搜寻所有的META-INF/spring.factories配置文件，并将其中org.springframework.boot.autoconfigure.EnableutoConfiguration对应的配置项通过反射（Java Refletion）实例化为对应的标注了@Configuration的JavaConfig形式的IoC容器配置类，然后汇总为一个并加载到IoC容器。**

- **流程图**

<img src="/Users/beifeng/Library/Application Support/typora-user-images/image-20201221201343939.png" alt="image-20201221201343939" style="zoom:100%;" />



​	