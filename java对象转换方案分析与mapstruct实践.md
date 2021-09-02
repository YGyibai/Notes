在Java日常开发中不可避免地涉及到各种对象的转换

### 常用方案分析

#### 1、fastJson

`CarDTO entity = JSON.parseObject(JSON.toJSONString(carDO), CarDTO.class);`

通过生成中间的json格式字符串，然后再转换为目标对象，性能非常差，因为中间会生成json格式字符串，如果转换过多，gc会非常频繁，如果两个实体类里属性名字不一致，会导致得不到预期的结果。同时对复杂场景支持能力不足，基本很少用。

#### 2、BeanUtil类

BeanUtil.copyProperties()结合手写get、set，对于简单的转换直接使用BeanUtil，复杂的转换自己手工写get、set。该方案的痛点就在于代码编写效率低、冗余繁杂还略显丑陋，并且BeanUtil因为使用了反射invoke去赋值性能不高。

只能使用数量较少、内容不多、转换不频繁的场景。

**apache.BeanUtils**

`org.apache.commons.beanutils.BeanUtils.copyProperties(do, entity);`

这种方案因为用到反射的原因，同时本身设计问题，性能比较差。集团开发规约明确规定禁止使用。

**Spring.BeanUtils**

`org.springframework.beans.BeanUtils.copyProperties(do, entity);`

这种方案针对apache的BeanUtils做了很多优化，整体性能提升不少，不过还是使用反射实现比不上原生代码处理，其次针对复杂场景支持能力不足。

#### 3、 beanCopier

```java
BeanCopier copier = BeanCopier.create(CarDO.class, CarDTO.class, false); copier.copy(do, dto, null);
```



这种方案动态生成一个要代理类的子类,其实就是通过字节码方式转换成性能最好的get和set方式,重要的开销在创建BeanCopier，整体性能接近原生代码处理，比BeanUtils要好很多，尤其在数据量很大时，但是针对复杂场景支持能力不足。

#### 4 、各种Mapping框架

**分类**



Object Mapping 技术从大的角度来说分为两类，一类是运行期转换，另一类则是编译期转换：



- 运行期反射调用 set/get 或者是直接对成员变量赋值。这种方式通过invoke执行赋值，实现时一般会采用beanutil, Javassist等开源库。运行期对象转换的代表主要是Dozer和ModelMaper。

- 编译期动态生成 set/get 代码的class文件，在运行时直接调用该class的 set/get 方法。该方式实际上仍会存在 set/get 代码，只是不需要开发人员自己写了。这类的代表是：MapStruct,Selma,Orika。



**分析**

- 无论哪种Mapping框架，基本都是采用xml配置文件 or 注解的方式供用户配置，然后生成映射关系。

- 编译期生成class文件方式需要DTO仍然有set/get方法，只是调用被屏蔽；而运行期反射方式在某些直接填充 field的方案中，set/get代码也可以省略。

- 编译期生成class方式会有源代码在本地，方便排查问题。

- 编译期生成class方式因为在编译期才出现java和class文件，所以热部署会受到一定影响。

- 反射型由于很多内容是黑盒，在排查问题时，不如编译期生成class方式方便。参考GitHub上工程java-object-mapper-benchmark可以看出主要框架性能比较。

- 反射型调用由于是在运行期根据映射关系反射执行，其执行速度会明显下降N个量级。

- 通过编译期生成class代码的方式，本质跟直接写代码区别不大，但由于代码都是靠模板生成，所以代码质量没有手工写那么高，这也会造成一定的性能损失。

综合性能、成熟度、易用性、扩展性，mapstruct是比较优秀的一个框架

### **Mapstruct使用指南**

关于mapstruct 配置的时候有一些坑，比如，无法生成实现类，出现java: No property named “XXX” exists in source parameter(s). Did you mean “null”等异常

主要错误是lombok,maven和mapstruct的版本不对应

下面是我测试能够使用的

```xml
 <properties>      
        <org.mapstruct.version>1.3.0.Final</org.mapstruct.version>
 </properties>
<dependencies>
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct</artifactId>
            <version>${org.mapstruct.version}</version>
        </dependency>
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct-jdk8</artifactId>
            <version>${org.mapstruct.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.10</version>
            <scope>provided</scope>
        </dependency>
</dependencies>
 <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <!-- depending on your project -->
                    <target>1.8</target>
                    <!-- depending on your project -->
                    <annotationProcessorPaths>
                        <path>
                            <groupId>org.mapstruct</groupId>
                            <artifactId>mapstruct-processor</artifactId>
                            <version>${org.mapstruct.version}</version>
                        </path>
                        <!-- other annotation processors -->
                        <path>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                            <version>1.18.10</version>
                        </path>
                    </annotationProcessorPaths>
                </configuration>
            </plugin>
        </plugins>

    </build>
```

**Mapstruct 和 lombok在build必须创建，不然idea无法编译它的注解**

**实体类**

```java
@Data
@Builder
public class Student {
	  private  String id;
    private String name;
    private Date birthday;
    private int[] numbers;
}


@Data
public class StudentDTO {
	  private  String id;
    private String name;
    private String birth;
    private int[] nums;
}

```

**转换类,编译后会在src自动生成.class文件**

```java
@Mapper(componentModel = "spring",imports = UUID.class)
public interface StuMapper {
//    StuMapper  INSTANCE= Mappers.getMapper(StuMapper.class);

    @Mappings({
            @Mapping(source = "id",target="id",defaultExpression = "java( UUID.randomUUID().toString() )"),
            @Mapping(source = "birthday",target = "birth",dateFormat = "yyyy-MM-dd HH:mm:ss"),
            @Mapping(source = "numbers",target = "nums")
    })
    StudentDTO studentDTO(Student student);

    List<StudentDTO> studentsToStudentDTOs(List<Student> students);
    @Mappings({
            @Mapping(source = "birthday",target = "birth",dateFormat = "yyyy-MM-dd HH:mm:ss"),
            @Mapping(source = "numbers",target = "nums")
    })
    void updateDTOFromStudent(Student student, @MappingTarget StudentDTO studentDTO);

    //逆向映射
    @InheritInverseConfiguration(name="updateDTOFromStudent")//表示值方法应继承相应反向方法的反向配置
    Student stuDTOToStudent(StudentDTO studentDTO);

    //逆向映射
    @InheritInverseConfiguration(name="studentDTO")//表示值方法应继承相应反向方法的反向配置
    Student stuDTOToStudent1(StudentDTO studentDTO);

}
```

**测试类**

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration({"classpath:applicationContext.xml"})
public class OtherTest {
    @Resource
    private StuMapper stuMapper;

    @Test
    public void arr(){
        Student student1 = Student.builder()
                .name("执行").birthday(new Date())
                .build();
        Student student2 = Student.builder()
                .name("小红").birthday(new Date())
                .build();
        List<Student> list=new ArrayList();
        list.add(student1);
        list.add(student2);
        List<StudentDTO> list1=stuMapper.studentsToStudentDTOs(list);
        for (StudentDTO studentDTO : list1) {
            System.out.println(studentDTO);
        }
    }

    @Test
    public void run() {
        int[] ints={1,2,3};
        Student student = Student.builder()
                .name("小明").birthday(new Date()).numbers(ints)
                .build();
        StudentDTO dto = stuMapper.studentDTO(student);
        System.out.println(dto);
        student=stuMapper.stuDTOToStudent(dto);
        System.out.println(student);
    }

    @Test
    public void updateDTOFromStudent(){
        int[] ints={1,2,3};
        Student student = Student.builder()
                .name("小明").birthday(new Date()).numbers(ints)
                .build();
        StudentDTO studentDTO = new StudentDTO();

        stuMapper.updateDTOFromStudent(student,studentDTO);
        System.out.println(student);
        System.out.println(studentDTO);
    }
}
```

@Mapper 只有在接口加上这个注解， MapStruct 才会去实现该接口
@Mapper 里有个 componentModel 属性，主要是指定实现类的类型，一般用到两个
    default：默认，可以通过 Mappers.getMapper(Class) 方式获取实例对象
    spring：在接口的实现类上自动添加注解 @Component，可通过 @Autowired 方式注入
@Mapping：属性映射，若源对象属性与目标对象名字一致，会自动映射对应属性
    source：源属性
    target：目标属性
    dateFormat：String 到 Date 日期之间相互转换，通过 SimpleDateFormat，该值为 SimpleDateFormat              的日期格式
    ignore: 忽略这个字段
@Mappings：配置多个@Mapping
@MappingTarget 用于更新已有对象
@InheritConfiguration 用于继承配置  name 属性用于配置哪个正向转换方式



**默认表达式是默认值和表达式的组合：**

@Mapper(componentModel = "spring",**imports = UUID.class**)

@Mapping(source = "id",target="id",defaultExpression = "java( UUID.randomUUID().toString() )"),

