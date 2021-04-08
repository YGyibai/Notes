### 添加maven依赖
```xml
        <!-- 集成swagger2  开始-->
        <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
        <!--   swagger2 结束-->
```
### 添加swagger配置
```java
    @Configuration
    @EnableSwagger2        ///开启swagger2
    public class SwaggerConfig {
    }
```


