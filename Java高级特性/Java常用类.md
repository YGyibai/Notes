# JAVA常用类

## 一、JavaAPI

### （Application Programming Interface 应用编程接口）文档

这些文档原先是程序中的注释。利用JavaDoc技术，将这些注释抽取出来，组织行程的以HTML为变现形式的API文档。

API中，包名以java开始的包是java核心包（javaCore Package）

​			  包名以javax开始的包是java扩展包（JavaExtension Package）

### 常用包

| 包名          | 内容                                                         |
| ------------- | ------------------------------------------------------------ |
| Java.applet.* | 提供了创建applet需要的所有类                                 |
| java.awt.*    | 提供了创建用户界面以及绘制和管理图形、图像的类               |
| java.io.*     | 提供了通过数据流、对象序列以及文件系统实现的系统输入、输出   |
| java.lang.*   | java编程语言的基本类库                                       |
| java.math.*   | 提供了简明的整数算术以及十进制算术的基本函数                 |
| Java.net.*    | 提供了用于实现网络通讯应用的所有类                           |
| Java.sql.*    | 提供了访问和处理来自于java标准数据源数据的类                 |
| java.text.*   | 包括一种独立于自然语言的方式处理文本、日期、数字和消息的类和接口 |
| Java.time.*   | 提供java日期、时间类（java8新增的包）                        |
| java.util.*   | 包括集合类、时间处理模式、日期时间工具等各类常用工具包       |

## 二、数字相关的类

### Java数字类

​	整数类	Short , 	Int ,	Long

​	浮点数类	Float,	Double

​	大数类	BigInteger(大整数)，	BigDecimal(大浮点数)

​	随机数类 	Random

​	工具类	Math

#### 大数字类

##### 	大整数类BigInteger

​			**支持无限大的整数运算**

```java
   BigInteger b1=new BigInteger("123456789");
        BigInteger  b2=new BigInteger("987654321");
        System.out.println("b1:"+b1+"\tb2:"+b2);
        System.out.println("加法操作："+b2.add(b1));
        System.out.println("减法操作："+b2.subtract(b1));
        System.out.println("乘法操作："+b2.multiply(b1));
        System.out.println("除法操作："+b2.divide(b1));
        System.out.println("最大数："+b2.max(b1));
        System.out.println("最小数："+b2.min(b1));
        BigInteger result[]=b2.divideAndRemainder(b1);//求出余数的除法操作
        System.out.println("商是："+result[0]+"\t余数是："+result[1]);
        int flag=b1.compareTo(b2);//比较操作
        if(flag==-1){
            System.out.println("b1<b2");
        }else if(flag==0){
            System.out.println("b1=b2");
        }else{
            System.out.println("b1>b2");
        }
```



##### 	大浮点数BigDecimal

​			**支持无限大的小数运算，注意精度和截断**

```java
BigDecimal b3=new BigDecimal("123456789.987654321");
BigDecimal b4=new BigDecimal("987654321.123456789");
//除了除法，其他方法和BigInteger的方法一样
System.out.println("除法操作："+b4.divide(b3,3,BigDecimal.ROUND_HALF_UP));

```

#### 随机数类

##### 	**Random随机数**

​			nextInt()	返回一个随机int

​			nextInt(int a)	返回一个【0，a】之间的随机int

​			nextDouble()	返回一个【0.0，1.0】之间double

​			ints方法批量返回随机数数组

```java
     //第一种方法，采用Random类，随机生成在int范围的随机数
        Random rd=new Random();
        System.out.println(rd.nextInt());
        System.out.println(rd.nextInt(100));//0-100之间的随机数
        System.out.println(rd.nextDouble());
        System.out.println(rd.nextLong());
        System.out.println("========================");
        //第二种方法，生成一个范围内的随机数，例如0-10之间的随机数
        System.out.println(Math.round(Math.random()*10));
        System.out.println("========================");
        //JDK1.8新增方法
        rd.ints();//返回无限个int类型范围内的数组
        int[] arr=rd.ints(10).toArray();//生成10个int范围的随机数
        for(int i=0;i<arr.length;i++){
            System.out.println(arr[i]);
        }
        System.out.println("=======================");
        arr=rd.ints(10).limit(5).toArray();//limit 生成10个随机数，限制为5个转换为数组
        for(int i=0;i<arr.length;i++){
            System.out.println(arr[i]);
        }
        System.out.println("==================");
        arr=rd.ints(5,10,100).toArray();
        /**
         * ints(streamSize,randomNumberOrigin,randomNumberBound)
         * streamSize,随机数个数
         * randomNumberOrigin,随机数原点，
         * randomNumberBound,随机数边界
         */
        for(int i=0;i<arr.length;i++){
            System.out.println(arr[i]);
        }
    }
```



##### 	Math.random()

​			返回一个【0.0，1.0】之间double

#### 数字工具类

​	绝对值函数 abs					对数函数log

​	比较函数max、min			幂函数pow

​	四舍五入函数round等

​	向下取整 floor				向上取整 ceil

```java
			  System.out.println(Math.abs(-5));//绝对值
        System.out.println(Math.max(-5,-8));//最大值
        System.out.println(Math.pow(-5,2));//幂函数
        System.out.println(Math.round(3.5));//四舍五入
        System.out.println(Math.ceil(3.5));//向上取整
        System.out.println(Math.floor(3.5));//向下取整
```

## 三、字符相关类

### 	String

​			Java中使用频率最高的类

​			是一个不可变对象，加减操作性能较差

​			以下方法需要牢记： 

​			返回类型  char：charAt,	indexOf,	

​			返回类型 boolean：equals ,	equalsIgnoreCase	contains	endsWith 	startsWith,	matches

​			返回类型 int：hashCode	length	split

​			返回类型String：replace	replaceAll	subString 	trim 	valueOf

​			matches() 方法用于检测字符串是否匹配给定的正则表达式。

​			Pattern.matches(regex, str)				regex为正则表达式

​			调用此方法的 str.matches(regex) 形式与以下表达式产生的结果完全相同：		        		

```java
String a="123,456,789;123";
        System.out.println(a.charAt(0));//返回第0个元素
           System.out.println(a.indexOf(";"));//返回第一个;的下表
           System.out.println(a.concat(";000"));//连接一个新字符串并返回，但源字符串不变
           System.out.println(a.contains("000"));//是否包含"000"
           System.out.println(a.equals("000"));//是否和000相等
           System.out.println(a.endsWith("000"));//是否以000结尾
           System.out.println(a.equalsIgnoreCase("000"));//在忽略大小写的情况下是否相等
           System.out.println(a.length());//返回字符串长度
           System.out.println(a.trim());//返回字符串去除前后空格后的字符串,但源字符串不变
           System.out.println("=================");
           String[] b=a.split(",");//将源字符串按照","分割成数组
           for(int i=0;i<b.length;i++){
               System.out.println(b[i]);
           }
           System.out.println("=================");
           System.out.println(a.substring(2,5));
           System.out.println(a.replace("1","a"));//将第一个"1"替换为"a"
           System.out.println(a.replaceAll("1","a"));//将全部1替换为a ，注意第一个参数为正则表达式
           
```



#### 	可变字符串

​			StringBuffer（字符串加减，同步，性能好）

​			StringBuilder（字符串加减，不同步，性能更好）

#### 	StringBuffer/StringBuilder：	方法一样，区别在同步

​			append/insert/delete/replace/substring

​			length	字符串实际大小，capacity字符串占用空间大小

​			trimToSize()：去除空隙，将字符串存储压缩到实际大小

​			如有大量append，实现预估大小，在调用相应构造函数

```java
StringBuffer sb=new StringBuffer("123456789");
        sb.append("000");
        System.out.println(sb);
        for(int i=0;i<sb.length();i+=4){
            if(i+4<sb.length()) {
                sb.insert(i + 3, ";");
            }else{
                break;
            }
        }
        System.out.println(sb);
        System.out.println(sb.delete(0,4));
        System.out.println(sb.substring(0,9));
```



## 四、时间相关类

### 	时间类

​		java.util.Date(基本废弃，Deprecated)

​		-getTime()，返回自1970.1.1以来的毫秒

​		java.sql.Date(和数据库对应的时间类)

​		Calendar是目前程序中最常用的，但是是抽象类

### 	Calendar

​			-get(Field) 来获取时间中每个属性的 值，注意，月份0-11

​			-getTime()，返回相应的Date对象

​			-getTimeInMillis()，返回自1970.1.1以来的毫秒

​			-set(Field) 设置时间字段

​			-add(field,amount)根据指定字段增加/减少时间

​			-roll(filed，amount)	根据制定字段增加/减少时间，但不影响上一级的时间段

```java
Calendar calendar=Calendar.getInstance();
    public void test1(){
        //get Year
        int year=calendar.get(calendar.YEAR);
        //get month 这里需要月份的范围为0-11，因此获取月份的时候需要+1
        int month =calendar.get(calendar.MONTH)+1;
        //get day
        int day=calendar.get(calendar.DAY_OF_MONTH);
        //get hour
        int hour=calendar.get(calendar.HOUR);
        int hour1=calendar.get(calendar.HOUR_OF_DAY);//24小时
        //get minute
        int minute=calendar.get(calendar.MINUTE);
        //get second
        int second =calendar.get(calendar.SECOND);
        //Week, Britain starts on Sunday   英国是从星期日开始计算
        int weekday=calendar.get(calendar.DAY_OF_WEEK);
        System.out.println("现在是"+year+"年"+month+"月"+day+"日"+hour+"时"+minute+"分"+second+"秒"+"星期"+weekday);
    }

    //一年后的今天
    public void test2(){
        // 同理换成下个月的今天 Calendar.add（calendar.MONTH,1）;
        calendar.add(calendar.YEAR,1);
        int year=calendar.get(calendar.YEAR);
        int month=calendar.get(calendar.MONTH)+1;
        int day=calendar.get(calendar.DAY_OF_MONTH);
        System.out.println("一年后的今天，"+year+"年"+month+"月"+day+"日");

    }


    //获取任意一个月的最后一天
    public void test3(){
        //假设求6月份的最后一天
        int currentMonth=6;
        //先求出7月份的第一天
        calendar.set(calendar.get(calendar.YEAR),currentMonth,1);
        calendar.add(calendar.DATE,-1);//Date 日期-1
        //获取日
        int day=calendar.get(calendar.DAY_OF_MONTH);
        System.out.println("6月份的最后一天为"+day+"号");
    }

    //设置日期
    public void test4(){
        calendar.set(calendar.YEAR,2000);
        System.out.println("现在是"+calendar.get(calendar.YEAR)+"年");
        calendar.set(2018,7,8);
        int     year=calendar.get(calendar.YEAR);
        int     month=calendar.get(calendar.MONTH)+1;
        int     day=calendar.get(calendar.DAY_OF_MONTH);
        System.out.println("现在是"+year+"年"+month+"月"+day+"日");
    }
```



### 	Java8时间包主要类

​			-LocalDate：日期类

​			-LocalTime：时间类（时分秒-纳秒）

​			-LocalDateTime：LocalDate+LocalTime

​			-Instant：时间戳

**LocalDate**

```java
//current time
        LocalDate today=LocalDate.now();
        System.out.println("current Date="+today);
        //根据指定时间创建LocalDate
        LocalDate firstDay_2014=LocalDate.of(2014, Month.JANUARY,1);
        System.out.println("Specific Date="+firstDay_2014);
        //2014年的第100天
        LocalDate hundredDay2014= LocalDate.ofYearDay(2014,100);
        System.out.println("100th day of 2014="+hundredDay2014);
        //从纪元日开始的365天
        LocalDate dateFromBase=LocalDate.ofEpochDay(365);
        System.out.println("365th day from base date="+dateFromBase);
```



## 五、格式化（Format）相关类

### 格式化类分类

​	**java.text包java.text.Format的子类**

​			NumberFormat：数字格式化，抽象类

​				DecimalFormat

​			MessageFormat：字符串格式化

​			DateFormat：日期/时间格式化，抽象类

​				SimpleDateFormat（线程不安全）

​							parse：将字符串格式化为事件对象

​							format：将时间对象格式化为字符串

​	**DecimalFormat示例**

```Java
  DecimalFormat df1,df2;
        System.out.println("整数部分为0的情况，0/#的区别");
        //整数部分为0，   #认为整数不存在，可不写；0认为没有，但至少写一位，写0
        df1=new DecimalFormat("#.00");
        df2=new DecimalFormat("0.00");
        System.out.println(df1.format(0.1));//.10
        System.out.println(df2.format(0.1));//0.10
        System.out.println("小数部分0/#的区别");
        //#代表最多有几位，0代表必须有且只能有几位
        df1 = new DecimalFormat("0.00");
        df2 = new DecimalFormat("0.##");

        System.out.println(df1.format(0.1)); // 0.10
        System.out.println(df2.format(0.1)); // 0.1

        System.out.println(df1.format(0.006)); // 0.01
        System.out.println(df2.format(0.006)); // 0.01

        System.out.println("整数部分有多位");
        //0和#对整数部分多位时的处理是一致的 就是有几位写多少位
        df2 = new DecimalFormat("#.00");
        df1=new DecimalFormat("0.00");
        System.out.println(df1.format(2)); // 2.00
        System.out.println(df2.format(2)); // 2.00


double pi=3.1415926;
        //取一位整数
        System.out.println(new DecimalFormat("0").format(pi));
        //取一位整数和两位小数
        System.out.println(new DecimalFormat("0.00").format(pi));
        //取两位整数和三位小数,整数不足部分以0填补     四舍五入
        System.out.println(new DecimalFormat("00.000").format(pi));
        //取所有的整数
        System.out.println(new DecimalFormat("#").format(pi));
        //以百分比方式计数，并取两位小数
        System.out.println(new DecimalFormat("#.##%").format(pi));


        long c=299792458;
        //显示为科学计数法，并取5位小数
        System.out.println(new DecimalFormat("#.#####E0").format(c));
        //显示为两位整数的科学计数法，并取四位小数
        System.out.println(new DecimalFormat("00.####E0").format(c));
        //每三位以逗号分开,000和,###无区别
        System.out.println(new DecimalFormat(",000").format(c));
        //将格式嵌入文本
        System.out.println(new DecimalFormat("光速大小为每秒，#米").format(c));
```

**MessageFormat**

```java
 String  message= "{0}{1}{2}{3}{4}{5}{6}{7}{8}{9}{10}{11}{12}{13}{14}{15}{16}";
        Object [] objects=new Object[]{"A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q"};
        String value= MessageFormat.format(message,objects);
        System.out.println(value);
        message="oh,{1,number,#.##} is a good s";
        objects=new Object[]{new Double(3.1415),new Integer(212)};
        value=MessageFormat.format(message,objects);
        System.out.println(value);
```

**SimpleDateFormat**

```java
 String str="2008-10-19 10:11:30.345";
        String pat1="yyyy-MM-dd HH:mm:ss.SSS";
        String pat2="yyyy年MM月dd日 HH时mm分ss秒SSS毫秒";
        //准备第一个模板，从字符串中提取出日期数字
        SimpleDateFormat sdf1=new SimpleDateFormat(pat1);
        //准备第二个模板，将提取后的字符串变为指定的格式
        SimpleDateFormat sdf2=new SimpleDateFormat(pat2);
        Date d=null;
        try {
            d=sdf1.parse(str);
            System.out.println(d);
        }catch (Exception e){
            e.printStackTrace();
        }
        System.out.println(sdf1.format(d));
        System.out.println(sdf2.format(d));
```



​	**java.time.format包下：**

​			DateTimeFormatter线程安全

​					ofPattern:设定时间格式

​					parse：将字符串格式化为时间对象

​					format：将时间对象格式化为字符串

**DateTimeFormatter**

```java
//将字符串转化为时间
        String  dateStr="2016年10月25日";
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日");//设置时间格式
        LocalDate date= LocalDate.parse(dateStr,formatter);
        System.out.println(date.getYear()+"-"+date.getMonthValue()+"-"+date.getDayOfMonth());
        System.out.println("================");
        //将日期转换为字符串输出
        LocalDateTime   now=LocalDateTime.now();
        DateTimeFormatter format=DateTimeFormatter.ofPattern("yyyy年MM月dd日 hh:mm:ss");
        String nowStr=now.format(format);
        System.out.println(nowStr);
```

