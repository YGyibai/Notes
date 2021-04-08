# 关键字Static和Final

## static关键字

**Java中static关键字（可作用在）**：方法、变量、类、匿名方法块。

**static变量：**不管有多少个对象，static修饰的变量在内存（栈）中只有一个。

**static方法：**可以用类名直接引用，无须new对象来引用，在同一类中可直接方法名调用

**static  块：**static块会在类加载的时候执行并且只会执行一次，并且static块 > 匿名块 >构造函数

**匿名块和构造函数，每new出来一次，就执行一次。**

### 单例模式

单例模式（Singleton）

限定某一个类在整个程序运行过程中，只能保留一个实例对象在内存空间（堆）中。

单例模式生成条件：保证一个类中只有一个对象

​	1.采用static来共享对象实例

​	2.采用private构造函数，防止外界new操作

Singleton例子：

```Java
 private static Singleton ton=new Singleton();
    private   String count;

    //定义一个私有的Singleton构造函数，防止外界new操作
    private Singleton(){
        this.count="我是count";
    }
    //set get方法
    public String getCount() {
        return count;
    }
    public void setCount(String count) {
        this.count = count;
    }
    public static Singleton getTon(){
        //静态方法可以使用静态变量，可以使用方法内的临时变量，但不能使用非静态成员变量
        return ton;
    }

    public static void main(String[] args) {
        //模仿外部类访问，本类还是可以进行new操作
        Singleton s=Singleton.getTon();
        System.out.println(s.getCount());
        Singleton s1=Singleton.getTon();
        System.out.println(s1.getCount());
        s1.setCount("我是S1");
        System.out.println(s.getCount());
        System.out.println(s1.getCount());
        System.out.println(s==s1);//true
        //本类    false
        System.out.println(new Singleton()==new Singleton());
    }
```

## Final关键字

**final关键字可以修饰：**类、方法、字段。

**final类：**没有子类继承。

**final方法：**不能被子类改写。

**final变量：**基本类型不能修改值，对象类型不能修改指针(new的对象相当于一个盒子，可以改变盒子里面的东西，但不能改变盒子)。

## 常量设计和常量池

### 常量：

**一种不会修改的变量**

Java中的常量： public static final

**//建议变量名字全大写，以连字符_相连**

**一种特殊的常量：接口内定义的变量默认是常量。**

**常量池：相同的值只存储一份，节省内存，共享访问**

### **基本类型的包装类**

Boolean、Byte、Short、Integer、Long、Character

Boolean： true	false

Byte,Character:  	\u000--\u007f（0---127）

Short，Int，Long：-128~127

Float，Double：没有缓存（常量池）

字符串常量

String s1=“add”;

### 基本类型的包装类和字符串有两种创建方式：

​	1.常量式（字面量）赋值创建，放在栈内存（将被常量化）

​	2.new对象进行创建，放在堆内存（不会常量化）

### 常量比较

#### 基本类型和包装类的比较：

基本类型和包装类比较，将对包装类自动拆箱

对象比较，比较地址

加法+会自动拆箱

```java 
Integer a=127;
    Integer a1=127;
    System.out.println(a==a1);//true    常量池 Integer范围在 -128~127
    Integer b=128;
    Integer b1=128;
    		System.out.println(b==b1);//false
		Integer a3=new Integer(100);
    Integer a4=new Integer(27);
    Integer c=new Integer(127);
        System.out.println(a==c);//false    基本类型和对象，比较其地址
				System.out.println(c==a3+a4);
//true  包装类相加会自动拆箱变为基本类型 127，基本类型和包装类比较自动拆装

    float f=0.5f;
    Float f1=0.5f;
    Float f2=0.5f;
        System.out.println(f==f1);//true    基本类型和包装类比较，包装类会自动拆箱
        System.out.println(f1==f2);//false  包装类比较，比较地址
	  	
```



#### String类的比较：

常量赋值（堆内存）和new创建（栈内存）不是同一个对象

编译器只会优化确定的字符串，并缓存

```java
 String s1="abc";
        String s2="abc";
        String s3= new String("abc");
        String s4=new String("abc");
        System.out.println(s1==s2);//true   两个都是常量
        System.out.println(s1==s3);//false   一个栈内存 一个堆内存
        String s5=s1+"def";//涉及到变量，故编译器不优化
        String s6="abc"+"def";
        String s7="abc"+new String("def");//涉及到new 对象 ，故编译器不优化
        System.out.println(s5==s6);//false  
        System.out.println(s6==s7);//false
```

## 不可变对象和字符串

### 不可变对象

#### 不可变对象（Immutable Object）

​	1.一旦创建，这个对象（状态/值）就不能被更改了

​	2.其内在的成员变量的值就不能修改了。

​	3.典型的不可变对象 ：

​					八个基本类型的包装类的对象 和基本类型

​					String，BIgInteger和BigDecimal 等的对象。

#### 可变对象（Mutable  Object）

普通对象 、StringBuffer  、StringBuild

#### 不可变对象（Immutable Object）的优点

​	1.只读，线程安全

​	2.并发读，提高性能

​	3.可以重复使用

#### 缺点

​	制造垃圾，浪费空间

#### 如何创建不可变对象

​		immutable 对象是不可改变的，有改变，请clone/new一个对象进行修改

​		所有属性都是final和private的

​		不提供setter方法

​		类是final的，或者所有的方法都是final

​		类中包含mutable对象，那么返回拷贝需要深度clone

### 字符串

#### 字符串

 字符串是Java使用最多的 类，是一种典型的不可变对象。

String定义有2种

​		String	a="abc";//常量赋值，栈分配内存

​		String	b=new String("abc");//new 对象，堆分配内存

字符串内容比较：equals方法

是否指向同一个对象：指针比较==

#### Java常量池（Constant Pool）

​	JDK1.7后，常量池从栈内存中转移到堆内存中

​	保存在编译期间就已经确定的数据

​	是一块特殊的内存

​	相同的常量字符串只存储一份，接省内存，共享访问

#### 字符串的加法

```java
String a="abc";
a=a+"def";//由于String不可修改，在常量池中又创建了一块内存（adcdef）而旧的内存只能等垃圾回收机制回收,浪费了内存，效率差

```

使用StringBuffer/StringBuild中的append方法进行修改
StringBuffer和StringBuild都是可变对象
**StringBuffer(同步，线程安全，修改快速)，**
**StringBuilder(不同步，线程不安全，修改更快）**

