[TOC]



# 一、初识Java

## Java开发环境配置

​	

```java
JDK安装目录设置：
		bin目录：存放编译、运行Java程序的可执行文件
		lib目录：存放Java的类库文件
		jre目录：存放Java运行环境文件
	检验Java是否成功安装 Java-version

变量设置参数如下：
    变量名：JAVA_HOME
    变量值：C:\Program Files (x86)\Java\jdk1.8.0_91        // 要根据自己jdk的实际路径配置
    变量名：CLASSPATH
    变量值：.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;   //记得前面有个"."
    变量名：Path
    变量值：%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
```



## 编写第一个Java程序

### 	Java程序执行过程

​		Java——编译器——.class文件——Java虚拟机——机器码（计算机）

### 	编写并运行Java文件步骤

​	

```
	a.创建Java程序，以.Java为扩展名
		b.编译源程序生成字节码文件。Java编译读取Java源程序并将其翻译成Java虚拟机 能够理解的指令集合，且以字节码的形式保存在文件中，以.class为文件名。  Javac 源文件 生成.class文件
		c.运行字节码文件。Java解释器读取字节码，取出指令并翻译成计算机能执行的代码，完成运行 Java 类名
```

# 二、数据类型与运算符

## 标识符的命名规则

```
1.标识符由大小写字母、数字、下划线（_）和美元符号（$）组成 _

_2.首字母以字母 、数字、下划线(_)和美元符号($)开头，但不能以数字开头 

3.标识符的命名不能与关键字、布尔值和null相同 

4.标识符区分大小写，没有长度限制，坚持见名知义的原则


```



## 关键字

| abstract | class    | final      | int       | public       | this      |
| -------- | -------- | ---------- | --------- | ------------ | --------- |
| assert   | continue | finally    | interface | return       | throw     |
| boolean  | default  | float      | long      | short        | throws    |
| break    | do       | for        | native    | static       | transient |
| type     | double   | if         | new       | strictfp     | try       |
| case     | else     | implements | package   | super        | void      |
| catch    | enum     | import     | private   | switch       | volatile  |
| char     | extends  | imstanceof | protected | synchronized | while     |

## 注释

```java
//单行注释
/*
*多行注释
*/
/**
*文档注释
*/

```

文档注释一般以@前缀，常用的文档注释标签如下

| 标签       | 含义         | 标签     | 含义                                   |
| ---------- | ------------ | -------- | -------------------------------------- |
| @author    | 作者名       | @version | 版本信息                               |
| @parameter | 参数及其意义 | @since   | 最早使用该方法 、类、接口的JDK版本信息 |
| @return    | 返回值       | @throws  | 异常类及抛出条件                       |

## 数据类型

#### 	

<img src="/Users/mac/Documents/素材/2.png" alt="1" style="zoom:80%;" />

### 基本数据类型及取值范围

```
数据存储是以“字节”（Byte）为单位，数据传输大多是以“位”（bit，又名“比特”）为单位，一个位就代表一个0或1（即二进制），每8个位（bit，简写为b）组成一个字节（Byte，简写为B），是最小一级的信息单位
```

| 基本类型 | 大小               | 取值范围                |
| -------- | ------------------ | ----------------------- |
| boolean  | 1字节8bit          | true/false              |
| byte     | 2字节8bit          | -128 ~ 127              |
| short    | 2字节16bit         | -32768 ~ 32767          |
| int      | 4字节32bit         | -2147483648 ~2147483647 |
| long     | 8字节64bit         | -2∧63 ~2∧63 -1          |
| char     | 2字节16Unicode字符 | 0 ~65535                |
| float    | 4字节32位浮点数    | -3.4E38 ~3.4E38         |
| double   | 8字节64位浮点数    | -1.7E308 ~ 1.7E308      |

### 变量

​	变量语法格式

​	【访问修饰符】变量类型 变量名【= 初始值】;

### 数据类型转换

#### 	1.算术运算时

​			存储位数越多，类型的级别越高。不同类型的运算，首先自动转化为表达式中最高级别的数据类型进行运算，运算结果依然是最高级别的数据类型，简称低接别自动转换为高级别。

<img src="/Users/mac/Documents/素材/1.png" alt="1" style="zoom:80%;" />

#### 	2.赋值运算时

​			a.自动类型转换			

​				将低级别类型赋值给高级别类型时将进行自动类型转换。

​				

```java
byte b=7;
int i=b; //b 自动转换为int
```

​			b.强制类型转换

​				将高级别的类型赋值给低级别类型时，必须进行强制类型转化。

```java
int mum=786;
byte by=(byte)num;//强制类型转换

```

## 运算符

1.赋值运算符

=   	+=		-=		*=		/=		%=

2.算术运算符

​	-			+			*		/			%			++

3.关系运算符

==		!=		>		<	<==		>==

4.逻辑运算符

&		|		^		！		&&	||

5.位运算符

&		|		^		~		<<		>>		>>

6.条件运算符

三目运算符

条件？表达式1：表达式2

首先对条件进行判断，如果结果为true返回表达式1的值

如果结果为flase，返回表达式2的值。

# 三、控制结构-选择结构和循环结构

## 流程控制结构

​	Java中有三种流程控制结构：顺序结构 、选择结构 、循环结构。



1. 顺序结构：**<u>程序从上向下执行每条语句的结构，中间没有判断和跳转</u>**。
2. 选择结构：程序根据<u>**条件判断的结果来选择执行不同的代码**</u>。选择结构可分为单分支结构、双分支结构和多分支结构。
3. 循环结构：程序根据<u>**判断条件来重复执行某段代码**</u>。Java提供了while、do-while、for语句来实现循环结构

## 选择结构

Java提供了if控制语句和switch语句来实现选择结构。

### if控制语句

if控制语句共有三种不同的形式，分别为单分支结构、双分支结构、多分支结构。

#### 		1）if语句单分支处理

​			语法格式：	

```java
      if(表达式){
          语句
      }
```

​			if是Java关键字，表达式是布尔型，其结果是true 或 false。

​			if语句的执行步骤：

​				a.对表达式的结果进行判断，

​				b.如果表达式的结果为真，则执行该语句

​				c.如果表达式的结果为假，则跳过该语句。

#### 		2）if-else双分支处理

​			if-else的语法格式：

```java
        if(表达式){
            语句1
        }else{
            语句2
        }
```

​				当表达式为真时，执行语句1，表达式为假时，执行语句2.

#### 			3）if多分支处理

​				多分支if语句语法结构：

```java
        if(表达式1){
            语句1
        }else if(表达式2){
            语句2
        }else{
            语句3
        }
```

​				其中else if语句可以有多个。

​				如果表达式1的结果为true，则执行语句1，否则对表达式2进行判断，

​				如果表达式2的结果为true，则执行语句2，否则执行语句3。

#### 			4）嵌套if语句

​					语法结构：						

```java
            if(表达式1){
              if(表达式2){
                语句1
              }else{
                语句2
              }
            }else{
              if(表达式3){
                语句3
              }else{
                语句4
              }
            }
            
```

### switch语句

​		Java中还提供了switch语句，用于实现多分支结构。

​			语法格式：

```java
switch(表达式){
	case 常量1:
		语句;
		break;
	case 常量2:
		语句;
		break;	
		....
	default:
		语句;
		break;
}
```

[^]: switch、case、break、default 是Java关键字。
[^]: switch后的表达式只能是整形、字符型或枚举型。
[^]: case用于与表达式进行匹配。
[^]: break用于终止后续语句的执行。
[^]: default 是可选的，当其他条件不匹配时执行default。

执行步骤：

​	1.计算switch表达式的值，

​	2.将计算结果从上到下依次与case常量的值比较，

​	3.如果相等执行该常量后的代码块，遇到break语句及跳出控制结构，

​	4.如果与任一case后常量都不匹配，则执行 default 中的代码块。

## 循环结构

​	Java中循环控制语句有while循环、do-while循环 、for循环等，循环结构的特点是在给定条件成立时，反复执行某代码块，直到条件不成立为止。

​	根据循环结构的定义可将它分为三个部分：

​		1.初始部分：设置循环的初始状态

​		2.循环体：需要重复执行的代码

​		3.循环条件：判断条件是否成立

### while 循环

​	语法格式：

```java
while(循环条件){
	循环体
}
```

[^注意]: while语句实现判断循环条件是否成立再执行循环体，如果第一次判断条件不成立，则循环体一次也不执行。

while语句执行步骤：

​	1.首先对循环条件进行判断，如果结果为真，则执行循环语句，

​	2.执行完毕后继续对循环条件进行判断，如果为真，继续执行循环语句

​	3.如果结果为假，则跳过循环结构执行后面的语句。

[^注意]: 不要忘了“i++”，他用来改变 循环变量的值，避免出现死循环。

### do-while循环

语法格式：

```java
do{
	循环体
}while(循环条件)；
```

[^注意]: do-while以分号结尾，分号不能省略。

do-while语句执行步骤：

​	1.首先执行循环语句，

​	2.执行完毕后继续对循环条件进行判断，如果为真，继续执行循环语句

​	3.如果结果为假，则跳过循环结构执行后面的语句。

[^注意]: do-while语句先执行循环体在进行条件判断，所以它至少执行了一次循环体
[^注意]: 不要忘了“i++”，他用来改变 循环变量的值，避免出现死循环。

### for循环

​	语法格式：

```java
for(表达式1;表达式2;表达式3){
	循环体
}
或
for(变量初始值;循环条件;修改循环变量的值){
	循环体
}
```

for循环语句的执行步骤：

1.首先执行表达式1，将循环变量初始化

2.然后执行表达式2，对循环条件进行判断

3.如果为真，则执行循环体

4.循环语句执行完毕后，执行表达式3，来改变循环变量的值，在执行表达式2，如果结果为真，继续循环

5.如果结果为假，则跳过循环结构执行后面的操作

[^提示]: 无论循环多少次，表达式1只执行一次。

for语句和while语句功能相似，都是先判断再执行，只是用了不同的语法方式。

### 多重循环

多重循环指一个循环语句中的循环体中再包含循环语句，又称嵌套循环。循环语句内可以嵌套多层循环，同时，不同的循环语句可以相互嵌套。

​	语法格式：

```
while(循环条件1){
	循环语句1
	for(循环条件2){
		循环语句2
    
  }
}
```

这是while语句和for语句嵌套的列子。其中while循环为外层循环，for循环为内层循环，因为是两层嵌套，所以称为二重循环

该二重循环的执行过程是：外层while循环每循环一次，内层for循环就从头到尾完整的执行一遍。

[^注意]: 代码缩进可以体现不同层次的代码结构，增加可读性。

​					关键代码应该有注释说明

### 循环对比

1.语法格式不同

2.执行顺序不同

​	while循环：先判断循环条件在执行循环体，如果条件不成立则跳出循环。

​	do-while循环：先执行循环体，在判断循环条件，循环体至少执行一次。

​	for循环：先执行变量初始化部分，在判断循环条件，然后执行循环体，最后进行循环变量的计算，如果条件不成立，跳出循环。

3.使用情况不同

​	对于循环次数确定的情况，通常选用for循环，对于循环次数不确定的情况，通常选用while循环和do-while循环语句。

## 跳转语句

​	JAVA语言中支持三种类型的跳转语句，break语句、continue语句、return语句，使用这些语句可以把控制转移到循环甚至程序的 其他部分

### break语句

break语句在循环中的作用是终止当前循环，在switch语句中的中作用是终止switch。

[^注意]: break语句只会出现在switch和循环语句中，没有其他使用场合。
[^提示]: Java中判断两个字符串是否相等时，用equals()方法判断是否相等，用”==“判断内存地址是否相等



### continue语句

​	continue语句作用是强制循环提前返回，也就是让循环跳过本次循环中的剩余代码，然后开始下一次循环。

[^提示]: 在while和do-while循环中，continue执行完后，程序将直接判断循环条件，而for循环则是先跳转到循环变量计算部分，然后判断循环条件。
[^注意]: continue语句只出现在循环语句这一场合。



### return语句

​	return语句作用是结束当前方法的 执行并退出返回到调用方法的语句处。

[^注意]: return又返回类型时，返回方法指定型的返回值，return没有返回类型时，用于方法的结束

# 四、数组

## 数组的定义及基本操作 

### 数组的定义

数组是用于储存多个相同类型数据的集合。

语法格式：

```java
数据类型[] 数组名=new 数据类型[数组长度];
或者
数据类型 数组名[]=new 数据类型[数组长度];
```

定义数组时一定要指定数组类型和数组名

数组类型用于确定分配的每个空间的大小

数组长度决定连续分配的空间的个数，可以通过数组的 length属性获取数组的长度

[] 表示定于了一个数组

#### 										数组元素分配的初始值

| 数组元素类型           | 默认初始值 |
| ---------------------- | ---------- |
| byte  short  int  long | 0          |
| float  double          | 0.0        |
| char                   | “\u0000”   |
| boolean                | Flase      |
| 引用类型               | null       |

### 数组元素的表示与赋值

由于数组在内存中是一段连续的空间， 数组元素在数组中顺序排列的编号也称为下标。

获取数组元素 并赋值的语法

```java
数组名[下标] //获取数组元素
数组名[小标]=值;//赋值
```

### 数组类型的初始化

语法如下

```
数据类型[] 数组名={值1，值2，值3，...};
或者
数据类型[] 数组名=new 数据类型[]{值1，值2，值3，...};
```

### 遍历数组

```java
int scores[]=new scores[5];
Scanner input = new Scanner(System.in);
for(int i=0;i<scores.length;i++){
  socers[i]=input.nextInt();
}
```

### 数组添加

```java
int index=-1;
String[] str={"max","min","avg",null};
		for(int  i=0;i<str.length;i++){
			  if(str[i]==null){
			    index=i;
			    System.out.println(index);
			    break;
			  }
		}
		if(index!=-1){
		    str[index]="num";
		    for(int i1=0;i1<str.length;i1++){
		    	System.out.println(str[i1]);        
		    }
		}else{
			System.out.println("数组已满");
		}
```

### 数组的修改

```java
int index=-1;
String[] str={"max","min","avg",null};
for (int i = 0; i < str.length; i++) {
			if (str[i].equals("min")) {
				index=i;
				break;
			}
		}
		if (index!=-1) {
			str[index]="mine";	
		}else {
			System.out.println("不存在min");
		}
```

### 数组删除

```java
int index=-1;
String[] str={"max","min","avg",null};
for (int i = 0; i < str.length; i++) {
			if (str[i].equals("mine")) {
				index=i;
				break;
			}
		}
		if (index!=-1) {
			for (int i = index; i < str.length-1; i++) {
				str[i]=str[i+1];
			}
			str[str.length-1]=null;
		}else {
			System.out.println("没有你要删除的内容");
		}
```

### 二维数组

语法格式

​	数据类型【】【】    数组名;

​	数据类型 数组名【】【】;

### 遍历二维数组

```java
		int[][] array=new int[][] {{80,60},{75,54},{77,59}};
		int toal;
		for (int i = 0; i < array.length; i++) {
				String str=i+1+"班";
      	toal=0;
        for (int j = 0; j < array[i].length; j++) {
          toal+=array[i][j];
        }
        System.out.println(str+"总成绩是："+toal);
		}
```

### Arrays类

JDK中提供了一个专门用于操作数组的工具类，即Arrays类

​										Arrays类常用方法

| 方法                    | 返回类型            | 说明                                       |
| ----------------------- | ------------------- | ------------------------------------------ |
| equals(array1,  array2) | Boolean             | 比较两个数组是否 相等                      |
| sort(array)             | Void                | 对数组array的元素进行升序排列              |
| toString(array)         | String              | 把一个数组array转换成一个字符串            |
| fill(array,val)         | Void                | 把数组array的所有元素都赋值为val           |
| copyOf(array,lenth)     | 与array数据类型一致 | 把数组 array复制成一个长度为length的新数组 |
| bianrySearch(array,val) | Int                 | 查询元素值val在数组array中的下标           |

# 六、面向对象基础

 

## 面向对象的基本概念

### 面向对象

面向对象的三大特征：封装、继承、多态。

### 对象

对象是用来描述客观事实的一个实体，由一组属性和方法构成。

对象是类的具体实例化，类是对象的抽象。

假如说类是一个模具，那么对象就是这个模具制作出来的东西。

### 类

类是具有相同属性和方法的一组集合。类定义了对象将会拥有的特征（属性）和行为（方法）

## 定义类

类的内部主要有属性和方法

语法格式如下

```
【访问修饰符】class 类名{

}
class是关键字
 类名首字母大写
```

### 属性

对象所拥有的特征在类中表示时称为类的属性

【访问修饰符】数据类型 属性名；

### 方法

对象执行操作的行为称为类的方法（函数）

【访问修饰符】返回类型 方法名称（参数类型 参数名）{

}

## 创建和使用对象

### 创建对象

类名 对象名=new 类名();

### 使用对象

对象名.属性		//引用对象的属性

对象名.方法名() 	//引用对象的方法

## 类的使用

## 成员方法

**类成员主要包含两个部分：成员方法、成员变量**

**类方法，类变量：用static修饰的方法和变量**

成员方法，成员变量只有在创建出对象后才能使用。

类方法，类变量是先于对象存在的，在类被加载到运行环境就存在了。可以用类名.方法或变量调用。

### 有参与无参的方法

```java
public void work(){
  无参的方法
}
public void work(int i){
  有参的方法
}
```

### 方法重载

#### 定义：

在一个类中可以定义多个同名的方法，但要求每个方法具有不同的参数类型和参数个数。

#### 特点：

1.同一个类中

2.方法名相同

3.具有不同的参数个数和类型

4.访问修饰符和返回值不能作为构成方法重载的依据

### 构造方法

定义：

主要作用是进行一些数据的初始化。

```
【访问修饰符】 方法名(参数列表){

}
```

特点

1.构造方法没有返回值

2.默认构造方法没有参数，因此可以重载，参数列表可选

3.构造方法的方法名和类名相同

## 成员变量

成员变量：直接在类中定义的变量，它定义在方法的外面

局部变量：定义在方法里的变量

### 成员变量与局部变量的区别

1.作用域不同。成员变量在同一个类中 成员方法中都可以使用，局部变量仅限于定义它的方法中。

2.初始值不同。成员变量有初始值，局部变量必须要定义赋值后在使用。

3.在不同的方法中可以有同名的局部变量

4.局部变量和成员变量可以同名，在统一方法中局部变量具有更高的优先级。

### 数据类型

变量的数据类型分为两种：基本数据类型和引用数据类型

引用数据类型分三种：类、数组、接口。

#### 基本数据类型和引用数据类型的区别

最主要的区别在于在内存中的存储方式不同，传递参数的方式不同

## 构造方法

语法：

```java
【访问修饰符】方法名(类名)（【参数列表】）{
		代码块
}
```

<!--注意：-->

1.方法名要和类名相同

2.构造方法没有返回值

3.默认构造方法没有参数，可以重载构造方法

### 构造方法重载

构造方法重载和方法重载一样，无返回值，当然同一个类中可以定义多个重载的构造方法

### This关键字

this关键字是对一个对象的默认引用，每个实例方法內部都有一个this引用变量，指向该方法的对象。

this使用如下：

1.解决成员变量和局部变量的同名冲突

```java
public void setName(String name){
  this.name=name;//成员变量和局部变量同名，必须使用this关键字
}

publid void SetSex(String sex){
  StudentSex=sex;
}

```

2.使用this调用成员方法

```java
public void play(int n){
	health=health-n;
	this.print();//this可以省略，直接调用print()方法
}
```

3.使用this调用重载的构造方法，只能在构造方法中使用，且必须在构造方法第一句

```java
public Pet(String name){
  this.name=name;
}
public Pet(String name,int age){
  this.Pet(name);
  this.age=age;
}
```





# 七、类的三大特征

## 封装

### 封装概述

Java中封装的实质：

​	1.将类的信息隐藏在类內部

​	2.不允许外部程序直接访问

​	3.访问通过该类提供的方法来实现对隐藏信息的操作和访问

封装的步骤

​	1.修改属性的可见性

​		将属性由public修改为private属性

​	2.设置set/get方法

​		可以手动设置set/get方法，也可以使用组合键ctrl+shift+s有系统添加

3.设置属性的存取限制

​		对属性进行合法性检查，利用条件判断语句进行赋值限制。

## 继承

### 概念：

新类可以在不增加自身代码的情况下通过现有的类中继承其属性及其方法，来充实自身内容，这种现象或行为就称为继承。新类为子类，现有的类称为父类。继承最基本的作用就是使得代码可以重用 ，增加软件的可扩充性。 

语法：

```java
【访问修饰符】class <SubClass> extends <SuperClass>{
  
}
```

在java中，子类可以继承父类以下内容：

​	1.可以继承public和protected修饰的属性和方法，不论子类和父类是否在同一包下

​	2.可以继承默认访问修饰符修饰的属性和方法，但子类和父类必须在同一个包下

​	3.无法继承父类的构造方法。

### 使用super关键字调用父类成员

语法：

```java
//访问父类构造方法
super(参数)
//访问父类属性和方法
super.<父类成员/方法>  
```

<!--注意-->

1.super只能出现在子类（子类的方法和构造方法）中

2.super用于访问父类的成员，如父类的属性、方法、构造方法等。

3.具有访问权限的限制，如：无法通过super访问父类的private成员。

### 实例化子类对象

子类继承父类时，构造方法的调用规则如下：

​	1.显示调用父类有参构造方法，将不执行无参构造方法，如果没有，系统默认调用父类的无参构造方法

​	2.子类的构造方法中通过this关键字显示调用了自身的其他方法，遵循以上规则

​	3.一旦提供了自定义构造方法，系统将不再提供这个默认构造方法，如果要使用它，必须手动添加。

### 方法重写

#### 方法重写必须满足如下要求：

​	1.二者具有相同的方法名

​	2.必须具有相同的参数列表

​	3.返回值类型相同

​	4.重写方法不能缩小被重写方法的访问权限。	

#### 重载（Overloading）和重写（Overriding）区别

​	1.重载涉及同一个类中的同名方法，要求方法名相同，参数列表不同，与返回值类型无关

​	2.重写涉及的是子类和父类之间的同名方法，要求方法名相同，参数列表相同，返回值类型相同

## 多态

在面向对象语言中，接口的多种不同的实现方式称为多态。

### 多态的作用：

1.以统一的接口来操作某一类中不同对象的动态行为

2.对象之间的解耦

### **多态存在的三个必要条件：**

**①继承 ②重写 ③父类引用指向子类对象**

### **多态的局限：**

**不能使用派生类特有的成员属性和派生类特有的成员方法。**

### 向上转型与向下转型

#### 向上转型

子类向父类的转换称为向上转型

语法：

```java
<父类型><引用变量>=new <子类型>();
```

##### 子类转向父类的规则

1.将一个父类的引用指向一个子类对象称为向上转型，系统会自动进行类型转换

2.此时通过父类引用变量调用的方法是子类覆盖或继承了父类的方法，不是父类方法

3.此时通过父类引用变量无法调用子类特有的方法。

#### 向下转型

语法：

```java
<子类型><引用变量名>=(<子类型>)父类型引用变量名;
```

### instanceof关键字

instanceof
instanceof是Java中的二元运算符，左边是对象，右边是类；当对象是右边类或子类所创建对象时，返回true；否则，返回false。

这里说明下：

类的实例包含本身的实例，以及所有直接或间接子类的实例

instanceof左边显式声明的类型与右边操作元必须是同种类或存在继承关系，也就是说需要位于同一个继承树，否则会编译错误
————————————————

其使用格式如下：    引用类型变量 instanceof （类、抽象类或接口）

### 多态的应用

#### 1.使用父类作为方法的形参

```java
//主人类
class Host{
  public void letCry(Animal a){
    a.cry();
  }
}
//以上为主人类代码，以下为调用代码
public Class Test{
  public static void main(String[] args){
    Host host=new Host();
    Animal animal;
    animal=new Dog();
    host.letCry(animal);//控制狗叫
    animal=new Cat();
    host.letCry(animal);//控制猫叫
    animal=new Brid(animal);
    host.letCry(animal);//控制鸟叫
    
  }
}
```

#### 2.使用父类作为方法的返回值

```java
calss Host{  
      public Animal doAnimal(String type){
      Animal animal;
      if(type=="dog"){
        animal=new Dog();
      }else if(type=="cat"){
        animal=new Cat();

      }else{
        animal=new Duck();
      }
  }
  //以上为主人类代码，以下为调用代码
  public class Test{
    public  static void main(String[] args){
      Host host=new Host();
      Animal animal;
      animal=host.doAnimal("dog");
      animal.cry();//狗叫
      animal=host.doAnimal("cat");
      animal.cry();//猫叫
    } 
  }
}
```



# 八、抽象

## 一、概述

当父类知道子类应该包含什么样的方法，但无法确定子类如何实现这些方法；在分析事物时，会发现事物的共性，将共性抽取出，实现的时候，就会有这样的情况：方法功能声明相同，但方法功能主体不同，这时，将方法声明抽取出，那么，此方法就是一个抽象方法。

### 1、抽象的定义格式

抽象方法的定义格式：public abstract 返回值类型 方法名（参数）；
抽象类的定义格式：abstract class 类名{}

### 2、抽象的特点

抽象类和抽象方法都需要被 abstract 修饰，抽象方法一定要定义在抽象类中
抽象不能直接创建对象，因为调用抽象方法没有意义
只有覆盖了抽象类中所有的抽象方法后，其子类才可以创建对象，否则该子类还是一个抽象类
之所以继承抽象类，更多的是在思想，是面对共性类型操作会更简单

### 3、抽象类的注意事项

1.抽象类一定是个父类，因为是不断抽取而来的
2.抽象类中可以不定义抽象方法，其存在的意义就是不让该类创建对象，方法可以直接让子类去使用
3.抽象关键字 abstract 不可以和以下关键字共存：
	private：私有的方法子类是无法继承到的，也不存在覆盖，如果 abstract 和 private 一起使用修饰方法， abstract 既要子类去实现这个方法，而 private 修饰子类根本无法得到父类这个方法，互相矛盾
	final：final 修饰的类不能被继承，而抽象类一定是父类
	static：static 修饰的表示静态的，不能被修改的，但可以直接被类所调用，而abstract修饰的是抽象的，即没有方法实体，也不能直接被调用

## 二、代码实例

先创建Develop.java父类，创建Develop抽象类，并创建抽象方法

 * 定义开发人员类，所有开发人员都具有工作的共性，

 * 对工作共性进行抽取，然后形成一个Develop类

 * 定义方法：工作

 * 抽象类不能实例化对象，即不能new，抽象方法没有主体，不能运行

 * 抽象类的使用：可以定义类继承抽象类，将抽象方法进行重写，创建子类的对象

```java
    public abstract class Develop {
    //定义工作方法
    //必须使用abstract关键字修饰
    //抽象的方法必须存在抽象类中，类也必须使用abstract关键字修饰
public abstract void work();
}
//java子类，重写父类的抽象方法
 
```

 定义Jsp开发人员

 * 继承抽象类Develop，重写抽象的方法


~~~java
```java
 public class Jsp extends Develop {
   //重写父类的抽象方法，去掉abstract关键字，加上方法主体
   public void work(){
       System.out.println("正在开发网页！");
   }
 }
   //在Main.java中调用
 public class Main {
     public static void main(String[] args){
         Php ph = new Jsp();
         ph.work();
     }
 }
~~~


实例解析

将共性“研发人员”抽取出来形成一个Develop类并定义方法：工作
抽象类不能实例化，即不能 new 抽象方法没有主体
可以定义类来继承抽象类，将抽象类进行重写，然后创建子类的对象
重写父类的抽象方法时，去掉abstract关键字，加上方法主体

# 九、接口

## 概述

接口表示的是功能的集合，可看做是一种数据类型，接口中全是抽象方法，没有普通方法，是比抽象更抽象的“类”，接口只描述应该具备的方法，并没有具体实现，具体的实现由接口的实现类（相当于接口的子类）来完成。这样将功能的定义与实现分离，优化了程序设计，解决了继承带来的耦合性，是一种只包含了功能声明的特殊类。

## 定义

```java
public interface 接口名{
    抽象方法1；
    抽象方法2；
    抽象方法3；
}
//使用interface代替了原来的class

```



## 接口特点

在类实现接口后，该类就会将接口中的抽象方法继承过来，此时该类需要重写该抽象方法，完成具体的逻辑
接口中定义功能，当需要有该功能时，可以让类实现该接口，接口只声明了应该具备的方法，是功能的声明
在具体实现类中重写方法，实现功能，是方法的具体实现
通过类实现接口将功能的声明与实现分离开来
接口中的方法均为公共访问的抽象方法
接口中无法定义普通的成员变量

## 接口中成员的特点

接口中可以定义变量，但是变量必须有固定的修饰符修饰，public static final  接口中的变量也称之为常量，其值不能改变，**接口中的变量默认有public static final修饰符**
eg：public static final int NUM = 1；        //NUM的值不能改变
**接口中可以定义方法，方法也有固定的修饰符：public abstract**
eg：public abstract void fun();
接口不可以创建对象
子类必须覆盖掉接口中所有的抽象方法后，子类才可以实例化，否则子类还是一个抽象类

## 接口的多实现

Java是不支持多继承的，因为当多个父类有相同的功能时，子类调用会产生不确定性，其核心原因就是在多继承父类中功能有主体，而导致调用运行时不确定运行哪个主体内容，采用接口的多实现很好的解决了这个问题，因为接口中没有方法体，由子类来明确

逻辑实例：

```java
interface Fu1
{
    void show1();
}
interface Fu2
{
    void show2();
}
class Zi implements Fu1,Fu2    //采用多实现，同时实现多个接口（Java中不支持多继承）
{
    public void show1(){}
    public void show2(){}
}
```

## 接口的多继承

类和类之间通过继承产生关系，接口和类之间通过实现产生关系，而接口和接口之间也是通过继承产生关系。多个接口之间使用 extends 进行继承。
在Java中是不支持多继承的，但接口可以多继承，其核心原因和接口的多实现一样
逻辑实例：

```java
interface Fu1
{
    void show1();
}
interface Fu2
{
    void show2();
}
interface Zi extends Fu1,Fu2
{
    void show3();
}
```

8、类继承类同时实现接口
子类通过继承父类拓展功能，如果子类想要继续拓展其他类中的功能，可以通过接口来完成

逻辑实例：

```java
class FU    //定义类
{
    public void show(){}
}
interface Inter    //定义接口
{
    public abstract void show1();
}
class Zi extends Fu implements Inter    //继承类同时实现接口
{
    public void show1(){}
}
```

## 接口的思想

接口的出现拓展了功能
接口其实就是暴露出来的规则
接口的出现降低的耦合性
接口的出现方便后期的维护

## 接口和抽象对比

### 接口和抽象的相同点：

都位于继承的顶端，用于被其他类实现或继承
都不能直接实例化对象

### 接口和抽象的区别：

抽象类为部分方法提供实现，避免子类重复使用这些方法，提高代码重用性；接口只能包含抽象方法
一个类只能继承一个直接父类（可能是抽象类），但可以实现多个接口（接口弥补了Java的单继承）
抽象类是这个事物中应该具备的内容
接口是这个事物中额外的内容
接口的实现类必须全部重写父类的抽象方法
抽象类可以不用全部重写父类的方法：
一个抽象类的子类可以是没有抽象方法的实体类，也可以是有抽象方法的抽象类，对于实体类就是全部覆盖了父类的抽象方法，抽象类则是在父类的基础上继续抽象，可以有更多的抽象方法，让子类实现的选择就更多
抽象类可以没有抽象方法，这样就和其他类没有区别，只是不能实例化

## 二者的选用：

优先选用接口，尽量少用抽象类
需要定义子类的行为，又要为子类提供共性功能时才选用抽象类

### 接口的成员特点

A：成员变量，只能是常量。默认修饰符为public、static、final

B：成员方法，只能是抽象方法。默认修饰符为public、abstract

**抽象类：**

1、抽象类使用abstract修饰；

2、抽象类不能实例化，即不能使用new关键字来实例化对象；

3、含有抽象方法（使用abstract关键字修饰的方法）的类是抽象类，必须使用abstract关键字修饰；

4、抽象类可以含有抽象方法，也可以不包含抽象方法，抽象类中可以有具体的方法；

5、如果一个子类实现了父类（抽象类）的所有抽象方法，那么该子类可以不必是抽象类，否则就是抽象类；

6、抽象类中的抽象方法只有方法体，没有具体实现；

**接口：**

1、接口使用interface修饰；

2、接口不能被实例化；

3、一个类只能继承一个类，但是可以实现多个接口；

4、接口中方法均为抽象方法；

5、接口中不能包含实例域或静态方法（静态方法必须实现，接口中方法是抽象方法，不能实现）

# 十、异常处理

异常：程序不正常的行为或状态。

### 异常分类

Throwable: 所有错误的祖先。

Error：系统内部错误或者资源耗尽。

Exception：程序有关的异常。

RuntimeException：程序自身的错误

非RuntimeException：外界相关的错误

#### 以编译器是否会辅助检查区分的异常：

1.Unchecked Exception：（编译器不会处理，需要程序员需要自己处理，以预防为主。）RuntimeException的子类

2.CheckedException：程序员必须处理，以发生后处理为主，编译器为辅助检查。	非RuntimeException（Exception的子类）

### 异常处理

Try-catch-finally:	一种保护代码正常运行的机制。

#### 异常结构

try - catch -finally

try -catch

try - finally

try必须有，catch、finally必须要有一个。

try：正常业务逻辑代码。

catch：当try发生异常时 ，将执行catch代码。若无异常，绕之。

fianlly：当try或catch执行结束后，必须执行fianlly代码块。

**方法存在可能异常的语句，但不处理，可以使用throws来声明异常**

### 自定义异常

**A 自定义异常，需要继承Exception或其子类：**

​		1.继承自Exception，就可以变成Checked Exception

​		2.继承自RuntimeException，就可以变成UnChecked Exception

**B 自定义重点在于构造函数**

​		1.调用父类Exception的message构造函数

​		2.可以自定义自己的成员变量

**C 在程序中Throw主动抛出异常**

**自定义异常**

```Java
  private String returnCode;
    private String  returnMsg;
    //构造方法
    public TestException(){
        super();
    }
    public TestException(String returnMsg){
        super(returnMsg);
        this.returnMsg=returnMsg;
    }
    public TestException(String returnCode,String returnMsg){
        super();
        this.returnMsg=returnMsg;
        this.returnCode=returnCode;
    }

    //返回信息，只用get方法
    public String getReturnCode() {
        return returnCode;
    }
    public String getReturnMsg() {
        return returnMsg;
    }
```

**测试类**

```java
 public static void exceptionStart() throws TestException{
        throw new TestException("12","NullPointException");
    }
    public static void main(String[] args){
        try {
            ExceptionTest.exceptionStart();
        }catch(TestException e){
            e.getStackTrace();
            System.out.println("returnCode:\n"+e.getReturnCode());
            System.out.println("returnMsg:\n"+e.getReturnMsg());
        }
    }
```

