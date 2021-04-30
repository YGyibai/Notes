WebDataBinder：将字符串形式的参数转化为服务端真正需要的类型的转换工具
> @ControllerAdvice 本质上也是一个Component，因此也会被当做组件被spring扫描到

```java
@Target(ElementType.TYPE)
@Retention(RetentionRolicy.RUNTIME)
@Documented
@Component
public @interface ControllerAdvice{}
```

