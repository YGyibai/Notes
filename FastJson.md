## FastJson

fastjson是一个java语言编写的高性能且功能完善的JSON库，它采用一种“假定有序快速匹配”的算法，把JSON Parse 的性能提升到了极致。它的接口简单易用，已经被广泛使用在缓存序列化，协议交互，Web输出等各种应用场景中。

### 常用API
fastjson API 入口类是**com.alibaba.fastjson.JSON**,常用的序列化操作都可以在JSON类上的静态方法直接完成。
```java
    public static final Object parse(String text); // 把JSON文本parse为JSONObject或者JSONArray 
    public static final JSONObject parseObject(String text)； // 把JSON文本parse成JSONObject    
    public static final <T> T parseObject(String text, Class<T> clazz); // 把JSON文本parse为JavaBean 
    public static final JSONArray parseArray(String text); // 把JSON文本parse成JSONArray 
    public static final <T> List<T> parseArray(String text, Class<T> clazz); //把JSON文本parse成JavaBean集合 
    public static final String toJSONString(Object object); // 将JavaBean序列化为JSON文本 
    public static final String toJSONString(Object object, boolean prettyFormat); // 将JavaBean序列化为带格式的JSON文本 
    public static final Object toJSON(Object javaObject); //将JavaBean转换为JSONObject或者JSONArray。
```
例子：
```java
    //简单java对象
    User user=new User("张三","123");
    System.out.println("简单java对象转换"+JSON.toJSONString(user));
    //集合转换为JSON字符串
    List<User> list= new ArrayList<>();
    User user1=new User("Logan","123");
    User user2=new User("king","123");
    list.add(user1);
    list.add(user2);
    String userList=JSON.toJSONString(list);
    System.out.println(userList);
    //复杂java类转换为JSON字符串
    UserGroup group= new UserGroup("userGroup",list);
    String userGroupString=JSON.toJSONString(group);
    System.out.println(userGroupString);
    //JSON字符串转换为List<T>对象
    list=JSON.parseArray(userList,User.class);
    list.forEach(System.out::println);
    //jSON字符串转换为复杂java对象
    group=JSON.parseObject(userGroupString, new TypeReference<UserGroup>(){});
    System.out.println(group);
```




	