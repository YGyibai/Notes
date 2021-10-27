# FastJson

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





# 关于JSON实际操作



## **2 错误原因**

**程序走到doAjaxDeleteRole方法利用\**list.get(0).getRoleid()\**取得list里的第一个对象的值的时候报错：```java.lang.ClassCastException: java.util.LinkedHashMap cannot be cast to xxx``，因为list里存放的不是RolePermission实体对象，而是LinkedHashMap，因此需要json转换。**



## **3 解决方式**

`从list中取出来的数据需要进行转化成json格式字符串，然后再将该json格式字符串转换成对象。`

### 3.1使用json-lib对其进行转换

pom.xml文件导入依赖

```xml
<dependency>	
  <groupId>net.sf.json-lib</groupId>
  <artifactId>json-lib</artifactId>
  <version>2.3</version>
  <classifier>jdk15</classifier>
</dependency>
```

 后台循环list代码：

```java
//遍历list
for(RolePermission rolePermission:list){    
    // 将list中的数据转成json字符串    
  		JSONObject jsonObject=JSONObject.fromObject(polePermission);    
    //将json转成需要的对象    
    RolePermission rolePermission = (RolePermission)JSONObject.toBean(jsonObject, RolePermission.class);
}
```

###  3.2使用fast-json对其进行转换

pom.xml文件导入依赖

```xml
<dependency>
<groupId>com.alibaba</groupId>
<artifactId>fastjson</artifactId>
<version>1.2.61</version>
</dependency>
```

java后台取值

```java
//遍历list
for(RolePermission rolePermission:list){
		// 将list中的数据转成json字符串
		String jsonObject=JSON.toJSONString(object);
		//将json转成需要的对象
		RolePermission rolePermission= JSONObject.parseObject(jsonObject,RolePermission.class);
}
```

