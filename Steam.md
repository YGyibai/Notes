### 概述
  Stream 是java8 中处理集合的关键抽象概念，可以对集合进行操作，可以执行非常复杂的查找、过滤和映射数据等操作。简单来说Stream是一种高效且易于使用的处理数据的方式。
  **流操作分为两种；**
    1.中间操作，每次返回一个新的流，可以有多个
    2.终端操作，每个流只能进行一次终端操作，终端操作结束后流无法再次使用，终端会产生一个新的集合或值
  **Stream特性：**
  - stream不存储数据，而是按照特定的规则对数据进行计算，一般会输出结果。
  - stream不会改变数据源，通常情况下会产生一个新的集合或一个值。
  - stream具有延迟执行特性，只有调用终端操作时，中间操作才会执行。

### 流的创建
  > Stream流分为**Stream(顺序流)**和**parallelStream(并行流)**两种创建方式
  > stream是顺序流，由主线程按顺序对流执行操作，而parallelStream是并行流，内部以多线程并行执行的方式对流进行操作，但> 前提是流中的数据处理没有顺序要求。如果流中的数据量足够大，并行流可以加快处速度。
  <font color="red">可以通过paralle()方法将顺序流转化成并行流</font>
  ```java
    Optional<Integer> findFirst = list.stream().parallel().filter(x->x>6).findFirst();
  ```

  **使用Collection的 stream()方法和parallelStream()方法**
  ```java
    List<String> list = new ArrayList<>();
    Stream<String> stream = list.stream(); //获取一个顺序流
    Stream<String> parallelStream = list.parallelStream(); //获取一个并行流
  ```
  **使用Arrays的stream()方法将数组转换为流**
  ```java
    Integer[] nums = new Integer[10];
    Stream<Integer> stream = Arrays.stream(nums);
  ```
  **使用Stream的静态方法 of() iterate() generate()**
  ```java
    Stream<Integer> stream = Stream.of(1,2,3,4,5,6);
    stream.forEach(System.out::println);//1 2 3 4 5 6
    Stream<Integer> stream2 = Stream.iterate(0, (x) -> x + 2).limit(6);
    stream2.forEach(System.out::println); // 0 2 4 6 8 10
    Stream<Double> stream3 = Stream.generate(Math::random).limit(2);
    stream3.forEach(System.out::println);
  ```
  **使用BufferedReader.lines()方法，将每行内容转成流**
  ```java
    BufferedReader reader = new BufferedReader(new FileReader("F:\\test_stream.txt"));
    Stream<String> lineStream = reader.lines();
    lineStream.forEach(System.out::println);
  ```
  **使用Pattern.compile()方法，将字符串分割成了流**
  ```java
    Pattern pattern=Pattern.compile(",");
    Stream<String> strStream= pattern.splitAsStream("a,b,c,d");
    strStream.forEach(System.out::print);
  ```

### 流的中间操作
  #### 切片和筛选
    filter：过滤流中的某些元素
    limit(n)：获取n个元素
    skip(n)：跳过n元素，配合limit(n)可实现分页
    distinct：通过流中元素的 hashCode() 和 equals() 去除重复元素
  ```java
    Stream<Integer> stream = Stream.of(6, 4, 6, 7, 3, 9, 8, 10, 12, 14, 14);
    Stream<Integer> newStream = stream.filter(s -> s > 5) //6 6 7 9 8 10 12 14 14
      .distinct() //6 7 9 8 10 12 14
      .skip(2) //9 8 10 12 14
      .limit(2); //9 8
    newStream.forEach(System.out::println);
  ```
  #### 映射
  map：接受一个函数作为参数，该函数应用到每一个元素上，并将其映射成一个新的元素
  flatMap：接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流。

  ```java
      List<String> list1 = Arrays.asList("1,2,3","a,b,c");
      //将每一个元素转换成一个新的且不带逗号的元素
      Stream<String> s1=list1.stream().map(s->s.replace(",",""));
      s1.forEach(System.out::println);
      Stream<String> s3=list1.stream().flatMap(s ->{
          //将每一个元素转换为流
          String[] split=s.split(",");
          Stream<String> s2=Arrays.stream(split);
          return s2;
      });
  ```
  #### 排序
  sorted()：自然排序，流中元素需要实现Comparable接口
  sorted(Comparator com)：定制排序，自定义Comparator排序器
  ```java
      User user1=new User("张三","1");
      User user2=new User("李四","2");
      User user3=new User("张三","3");
      User user4=new User("王五","4");
      List<User> users=Arrays.asList(user4,user1,user2,user3);
      //自定义排序，先按名字排序，在按年龄排序
      users.stream().sorted(
              (o1,o2)->{
                  if (o1.getUserName().equals(o2.getUserName()))
                      return Integer.parseInt(o1.getPassWord()) - Integer.parseInt(o2.getPassWord());
                  else
                      return o1.getUserName().compareTo(o2.getUserName());
              }
      ).forEach(System.out::println);
  ```
  #### 消费（变更属性）
  peek：如同于map，能得到流中的每一个元素。但map接收的是一个Function表达式，有返回值；而peek接收的是Consumer表达式，没有返回值。
  ```java
    Student s1 = new Student("aa", 10);
    Student s2 = new Student("bb", 20);
    List<Student> studentList = Arrays.asList(s1, s2);
    studentList.stream()
            .peek(o -> o.setAge(100))
            .forEach(System.out::println);   
    //结果：
    Student{name='aa', age=100}
    Student{name='bb', age=100}            
  ```

### 流的终端操作
  #### 匹配 聚合操作
  allMath：接受一个Predicate函数，当流中每个元素都符合断言时才返回true，否则返回false
  noneMatch：接收一个 Predicate 函数，当流中每个元素都不符合该断言时才返回true，否则返回false
  anyMatch：接收一个 Predicate 函数，只要流中有一个元素满足该断言则返回true，否则返回false
        findFirst：返回流中第一个元素
        findAny：返回流中的任意元素
        count：返回流中元素的总个数
        max：返回流中元素最大值
        min：返回流中元素最小值




### 实例
  #### 遍历和匹配(forEach/find/match)
  Stream也是支持类似集合的遍历和匹配元素的，只是Stream中的元素是以Optional类型存在的。Stream的遍历、匹配非常简单。

  ```java
      List<Integer> list = Arrays.asList(7, 6, 9, 3, 8, 2, 1);
      //输出符合遍历条件的数据
      list.stream().filter(x->x>5).forEach(System.out::println);
      // 匹配第一个
      Optional<Integer> findFirst = list.stream().filter(x -> x > 6).findFirst();
      // 匹配任意（适用于并行流）
      Optional<Integer> findAny = list.parallelStream().filter(x -> x > 6).findAny();
      // 是否包含符合特定条件的元素 断言asset
      boolean anyMatch = list.stream().anyMatch(x -> x < 0);
      System.out.println("匹配第一个值：" + findFirst.get());
      System.out.println("匹配任意一个值：" + findAny.get());
      System.out.println("是否存在大于6的值：" + anyMatch);
  ```
  #### 筛选(filter)
  筛选，是按照一定的规则校验流中的元素，将符合条件的元素提取到新的流中的操作。
  案例一：筛选出Integer集合中大于7的元素，并打印出来
  ```java
    public class StreamTest {
      public static void main(String[] args) {
        List<Integer> list = Arrays.asList(6, 7, 3, 8, 1, 2, 9);
        Stream<Integer> stream = list.stream();
        stream.filter(x -> x > 7).forEach(System.out::println);
      }
    }
  ```
  案例二： 筛选员工中工资高于8000的人，并形成新的集合。 形成新集合依赖collect（收集），后文有详细介绍。
  ```java
      List<Person> personList = new ArrayList<Person>();
      personList.add(new Person("Tom", 8900, 23, "male", "New York"));
      personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
      personList.add(new Person("Lily", 7800, 21, "female", "Washington"));
      personList.add(new Person("Anni", 8200, 24, "female", "New York"));
      personList.add(new Person("Owen", 9500, 25, "male", "New York"));
      personList.add(new Person("Alisa", 7900, 26, "female", "New York"));
      List<String> fiterList = personList.stream().filter(x -> x.getSalary() > 8000).map(Person::getName)
          .collect(Collectors.toList());
      System.out.print("高于8000的员工姓名：" + fiterList);
  ```
 #### 聚合(max/min/count)
  max/min/count在mysql中我们常用它们进行数据统计。Java stream中也引入了这些概念和用法，极大地方便了我们对集合、数组的数据统计工作。
  案例一：获取String集合中最长的元素。
  ```java
    List<String> list = Arrays.asList("adnm", "admmt", "pot", "xbangd", "weoujgsd");
		Optional<String> max = list.stream().max(Comparator.comparing(String::length));
		System.out.println("最长的字符串：" + max.get());
  ```
  案例二：获取Integer集合中的最大值。
  ```java
    List<Integer> list = Arrays.asList(7, 6, 9, 4, 11, 6);
		// 自然排序
		Optional<Integer> max = list.stream().max(Integer::compareTo);
		// 自定义排序
		Optional<Integer> max2 = list.stream().max(new Comparator<Integer>() {
			@Override
			public int compare(Integer o1, Integer o2) {
				return o1.compareTo(o2);
			}
		});
		System.out.println("自然排序的最大值：" + max.get());
		System.out.println("自定义排序的最大值：" + max2.get());
  ```
  案例三：获取员工工资最高的人。
  ```java
    List<Person> personList = new ArrayList<Person>();
		personList.add(new Person("Tom", 8900, 23, "male", "New York"));
		personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
		personList.add(new Person("Lily", 7800, 21, "female", "Washington"));
		personList.add(new Person("Anni", 8200, 24, "female", "New York"));
		personList.add(new Person("Owen", 9500, 25, "male", "New York"));
		personList.add(new Person("Alisa", 7900, 26, "female", "New York"));
		Optional<Person> max = personList.stream().max(Comparator.comparingInt(Person::getSalary));
		System.out.println("员工工资最大值：" + max.get().getSalary());
  ```
  案例四：计算Integer集合中大于6的元素的个数。
  ```java
    List<Integer> list = Arrays.asList(7, 6, 4, 8, 2, 11, 9);
		long count = list.stream().filter(x -> x > 6).count();
		System.out.println("list中大于6的元素个数：" + count);
  ```
  #### 映射(map/flatMap)
  映射，可以将一个流的元素按照一定的映射规则映射到另一个流中。分为map和flatMap：
  - map：接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
  - flatMap：接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流。

 案例一：英文字符串数组的元素全部改为大写。整数数组每个元素+3。
  ```java
    String[] strArr = { "abcd", "bcdd", "defde", "fTr" };
		List<String> strList = Arrays.stream(strArr)
    .map(String::toUpperCase)
    .collect(Collectors.toList());
		List<Integer> intList = Arrays.asList(1, 3, 5, 7, 9, 11);
		List<Integer> intListNew = intList.stream().map(x -> x + 3).collect(Collectors.toList());
		System.out.println("每个元素大写：" + strList);
		System.out.println("每个元素+3：" + intListNew);
  ```

  案例二：将员工的薪资全部增加1000。
  ```java
		List<Person> personList = new ArrayList<Person>();
		personList.add(new Person("Tom", 8900, 23, "male", "New York"));
		personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
		personList.add(new Person("Lily", 7800, 21, "female", "Washington"));
		personList.add(new Person("Anni", 8200, 24, "female", "New York"));
		personList.add(new Person("Owen", 9500, 25, "male", "New York"));
		personList.add(new Person("Alisa", 7900, 26, "female", "New York"));

		// 不改变原来员工集合的方式
		List<Person> personListNew = personList.stream().map(person -> {
			Person personNew = new Person(person.getName(), 0, 0, null, null);
			personNew.setSalary(person.getSalary() + 10000);
			return personNew;
		}).collect(Collectors.toList());
		System.out.println("一次改动前：" + personList.get(0).getName() + "-->" + personList.get(0).getSalary());
		System.out.println("一次改动后：" + personListNew.get(0).getName() + "-->" + personListNew.get(0).getSalary());

		// 改变原来员工集合的方式
		List<Person> personListNew2 = personList.stream().map(person -> {
			person.setSalary(person.getSalary() + 10000);
			return person;
		}).collect(Collectors.toList());
		System.out.println("二次改动前：" + personList.get(0).getName() + "-->" + personListNew.get(0).getSalary());
		System.out.println("二次改动后：" + personListNew2.get(0).getName() + "-->" + personListNew.get(0).getSalary());
  ```

#### 归约(reduce)

归约，也称缩减，顾名思义，是把一个流缩减成一个值，能实现对集合求和、求乘积和求最值操作。

案例一：求Integer集合的元素之和、乘积和最大值。
```java
    List<Integer> list = Arrays.asList(1, 3, 2, 8, 11, 4);
		// 求和方式1
		Optional<Integer> sum = list.stream().reduce((x, y) -> x + y);
		// 求和方式2
		Optional<Integer> sum2 = list.stream().reduce(Integer::sum);
		// 求和方式3
		Integer sum3 = list.stream().reduce(0, Integer::sum);
		
		// 求乘积
		Optional<Integer> product = list.stream().reduce((x, y) -> x * y);

		// 求最大值方式1
		Optional<Integer> max = list.stream().reduce((x, y) -> x > y ? x : y);
		// 求最大值写法2
		Integer max2 = list.stream().reduce(1, Integer::max);

		System.out.println("list求和：" + sum.get() + "," + sum2.get() + "," + sum3);
		System.out.println("list求积：" + product.get());
		System.out.println("list求和：" + max.get() + "," + max2);  
```

案例二：求所有员工的工资之和和最高工资。
```java
		List<Person> personList = new ArrayList<Person>();
		personList.add(new Person("Tom", 8900, 23, "male", "New York"));
		personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
		personList.add(new Person("Lily", 7800, 21, "female", "Washington"));
		personList.add(new Person("Anni", 8200, 24, "female", "New York"));
		personList.add(new Person("Owen", 9500, 25, "male", "New York"));
		personList.add(new Person("Alisa", 7900, 26, "female", "New York"));

		// 求工资之和方式1：
		Optional<Integer> sumSalary = personList.stream().map(Person::getSalary).reduce(Integer::sum);
		// 求工资之和方式2：
		Integer sumSalary2 = personList.stream().reduce(0, (sum, p) -> sum += p.getSalary(),
				(sum1, sum2) -> sum1 + sum2);
		// 求工资之和方式3：
		Integer sumSalary3 = personList.stream().reduce(0, (sum, p) -> sum += p.getSalary(), Integer::sum);

		// 求最高工资方式1：
		Integer maxSalary = personList.stream().reduce(0, (max, p) -> max > p.getSalary() ? max : p.getSalary(),
				Integer::max);
		// 求最高工资方式2：
		Integer maxSalary2 = personList.stream().reduce(0, (max, p) -> max > p.getSalary() ? max : p.getSalary(),
				(max1, max2) -> max1 > max2 ? max1 : max2);

		System.out.println("工资之和：" + sumSalary.get() + "," + sumSalary2 + "," + sumSalary3);
		System.out.println("最高工资：" + maxSalary + "," + maxSalary2);
```
#### 收集(collect)
collect主要依赖java.util.stream.Collectors类内置的静态方法。
  ##### 归集(toList/toSet/toMap)
  ```java
    List<Integer> list = Arrays.asList(1, 6, 3, 4, 6, 7, 9, 6, 20);
		List<Integer> listNew = list.stream().filter(x -> x % 2 == 0).collect(Collectors.toList());
		Set<Integer> set = list.stream().filter(x -> x % 2 == 0).collect(Collectors.toSet());

		List<Person> personList = new ArrayList<Person>();
		personList.add(new Person("Tom", 8900, 23, "male", "New York"));
		personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
		personList.add(new Person("Lily", 7800, 21, "female", "Washington"));
		personList.add(new Person("Anni", 8200, 24, "female", "New York"));
		
		Map<?, Person> map = personList.stream().filter(p -> p.getSalary() > 8000)
				.collect(Collectors.toMap(Person::getName, p -> p));
		System.out.println("toList:" + listNew);
		System.out.println("toSet:" + set);
		System.out.println("toMap:" + map);
  ```
  ##### 统计(count/averaging)
  Collectors提供了一系列用于数据统计的静态方法：

  > 计数：count
  > 平均值：averagingInt、averagingLong、averagingDouble
  > 最值：maxBy、minBy
  > 求和：summingInt、summingLong、summingDouble
  > 统计以上所有：summarizingInt、summarizingLong、summarizingDouble
  案例：统计员工人数、平均工资、工资总额、最高工资。
  
	```java
		List<Person> personList = new ArrayList<Person>();
		personList.add(new Person("Tom", 8900, 23, "male", "New York"));
		personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
		personList.add(new Person("Lily", 7800, 21, "female", "Washington"));

		// 求总数
		Long count = personList.stream().collect(Collectors.counting());
		// 求平均工资
		Double average = personList.stream().collect(Collectors.averagingDouble(Person::getSalary));
		// 求最高工资
		Optional<Integer> max = personList.stream().map(Person::getSalary).collect(Collectors.maxBy(Integer::compare));
		// 求工资之和
		Integer sum = personList.stream().collect(Collectors.summingInt(Person::getSalary));
		// 一次性统计所有信息
		DoubleSummaryStatistics collect = personList.stream().collect(Collectors.summarizingDouble(Person::getSalary));
	
		System.out.println("员工总数：" + count);
		System.out.println("员工平均工资：" + average);
		System.out.println("员工工资总和：" + sum);
		System.out.println("员工工资所有统计：" + collect);
  ```java
  ##### 分组(partitioningBy/groupingBy)
  分区：将stream按条件分为两个Map，比如员工按薪资是否高于8000分为两部分。
  分组：将集合分为多个Map，比如员工按性别分组。有单级分组和多级分组。

  案例：将员工按薪资是否高于8000分为两部分；将员工按性别和地区分组
  ```java
    List<Person> personList = new ArrayList<Person>();
    personList.add(new Person("Tom", 8900, "male", "New York"));
    personList.add(new Person("Jack", 7000, "male", "Washington"));
    personList.add(new Person("Lily", 7800, "female", "Washington"));
    personList.add(new Person("Anni", 8200, "female", "New York"));
    personList.add(new Person("Owen", 9500, "male", "New York"));
    personList.add(new Person("Alisa", 7900, "female", "New York"));

    // 将员工按薪资是否高于8000分组
    Map<Boolean, List<Person>> part = personList.stream().collect(Collectors.partitioningBy(x -> x.getSalary() > 8000));
    // 将员工按性别分组
    Map<String, List<Person>> group = personList.stream().collect(Collectors.groupingBy(Person::getSex));
    // 将员工先按性别分组，再按地区分组
    Map<String, Map<String, List<Person>>> group2 = personList.stream().collect(Collectors.groupingBy(Person::getSex, Collectors.groupingBy(Person::getArea)));
    System.out.println("员工按薪资是否大于8000分组情况：" + part);
    System.out.println("员工按性别分组情况：" + group);
    System.out.println("员工按性别、地区：" + group2);
  ```
  ##### 接合(joining)
  joining可以将stream中的元素用特定的连接符（没有的话，则直接连接）连接成一个字符串。
  ```java
      List<Person> personList = new ArrayList<Person>();
      personList.add(new Person("Tom", 8900, 23, "male", "New York"));
      personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
      personList.add(new Person("Lily", 7800, 21, "female", "Washington"));

      String names = personList.stream().map(p -> p.getName()).collect(Collectors.joining(","));
      System.out.println("所有员工的姓名：" + names);
      List<String> list = Arrays.asList("A", "B", "C");
      String string = list.stream().collect(Collectors.joining("-"));
      System.out.println("拼接后的字符串：" + string)
  ```
  ##### 归约(reducing)
  Collectors类提供的reducing方法，相比于stream本身的reduce方法，增加了对自定义归约的支持。
  ```java
    List<Person> personList = new ArrayList<Person>();
    personList.add(new Person("Tom", 8900, 23, "male", "New York"));
    personList.add(new Person("Jack", 7000, 25, "male", "Washington"));
    personList.add(new Person("Lily", 7800, 21, "female", "Washington"));

    // 每个员工减去起征点后的薪资之和（这个例子并不严谨，但一时没想到好的例子）
    Integer sum = personList.stream().collect(Collectors.reducing(0, Person::getSalary, (i, j) -> (i + j - 5000)));
    System.out.println("员工扣税薪资总和：" + sum);

    // stream的reduce
    Optional<Integer> sum2 = personList.stream().map(Person::getSalary).reduce(Integer::sum);
    System.out.println("员工薪资总和：" + sum2.get());
  ```
#### 排序(sorted)
  sorted，中间操作。有两种排序：
    sorted()：自然排序，流中元素需实现Comparable接口
    sorted(Comparator com)：Comparator排序器自定义排序
  案例：将员工按工资由高到低（工资一样则按年龄由大到小）排序
  ```java
    List<Person> personList = new ArrayList<Person>();

		personList.add(new Person("Sherry", 9000, 24, "female", "New York"));
		personList.add(new Person("Tom", 8900, 22, "male", "Washington"));
		personList.add(new Person("Jack", 9000, 25, "male", "Washington"));
		personList.add(new Person("Lily", 8800, 26, "male", "New York"));
		personList.add(new Person("Alisa", 9000, 26, "female", "New York"));

		// 按工资升序排序（自然排序）
		List<String> newList = personList.stream().sorted(Comparator.comparing(Person::getSalary)).map(Person::getName)
				.collect(Collectors.toList());
		// 按工资倒序排序
		List<String> newList2 = personList.stream().sorted(Comparator.comparing(Person::getSalary).reversed())
				.map(Person::getName).collect(Collectors.toList());
		// 先按工资再按年龄升序排序
		List<String> newList3 = personList.stream()
				.sorted(Comparator.comparing(Person::getSalary).thenComparing(Person::getAge)).map(Person::getName)
				.collect(Collectors.toList());
		// 先按工资再按年龄自定义排序（降序）
		List<String> newList4 = personList.stream().sorted((p1, p2) -> {
			if (p1.getSalary() == p2.getSalary()) {
				return p2.getAge() - p1.getAge();
			} else {
				return p2.getSalary() - p1.getSalary();
			}
		}).map(Person::getName).collect(Collectors.toList());

		System.out.println("按工资升序排序：" + newList);
		System.out.println("按工资降序排序：" + newList2);
		System.out.println("先按工资再按年龄升序排序：" + newList3);
		System.out.println("先按工资再按年龄自定义降序排序：" + newList4);
  ```
#### 提取/组合
流也可以进行合并、去重、限制、跳过等操作。
```java
  String[] arr1 = { "a", "b", "c", "d" };
  String[] arr2 = { "d", "e", "f", "g" };

  Stream<String> stream1 = Stream.of(arr1);
  Stream<String> stream2 = Stream.of(arr2);
  // concat:合并两个流 distinct：去重
  List<String> newList = Stream.concat(stream1, stream2).distinct().collect(Collectors.toList());
  // limit：限制从流中获得前n个数据
  List<Integer> collect = Stream.iterate(1, x -> x + 2).limit(10).collect(Collectors.toList());
  // skip：跳过前n个数据
  List<Integer> collect2 = Stream.iterate(1, x -> x + 2).skip(1).limit(5).collect(Collectors.toList());

  System.out.println("流合并：" + newList);
  System.out.println("limit：" + collect);
  System.out.println("skip：" + collect2);
```