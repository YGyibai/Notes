# XML：可扩展标记型语言

## 一、基本概念

eXtensible Markup Language:可扩张标记型语言

 		标记型语言： HTML是标记语言，即使用标签来操作

​		 可扩展：

​				 HTML 里面的标签是固定的，每个标签都有特定的含义

​				 xml标签可以自己定义，可以写中文的标签<猫></猫>

## 二、xml用途

​		1.不同系统之间传输数据

​		2.充当小型数据库

​		3.经常用在配置文件，如：配置MySQL数据库

## 三、语法

 xml文档后缀名为：.xml

### 文档声明：

创建一个xml文件后，第一步就是必须要有一个文档声明

```java
<?xml version ="1.0" encoding="UTF-8"?>
```

**version：**xml版本，必须写

**encoding：**xml编码 	（保存时编码和设置打开时候的编码需要一致，否则会出现乱码）

**standalone：**是否需要依赖其他文件 yes/no

### 标签的定义

**注意事项：**

1.有始有终：<person></person>

2.合理嵌套：<aa><bb></bb></aa>

3.空格和换行当做内容来解析，所以我们需要注意一些缩进的问题

**名称规则：**

1.xml代码区分大小写

2.名称不能以数字或者标点符号开始

3.不能以xml、XML、Xml等开头

4.不能包含空格和冒号

### 属性的定义

1.一个标签上可以有多个属性<person id="1" name="sk"></person>

2.属性名和值之间使用 = 连接，属性值用引号包起来（单引号和双引号都可以）

### 注释

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- xml 注释  -->
```

注释不能嵌套，并且不能放到第一行，第一行必须放文档声明

### 特殊字符

　利用转义字符

| 特殊符号 | 预定义实体 | 特殊符号 | 预定义实体 |
| -------- | ---------- | -------- | ---------- |
| &        | &amp;amp;  | "        | &quot ;    |
| <        | &lt ;      | '        | &apos      |
| >        | &gt ;      |          |            |

### CDATA区

可以解决多个字符都需要转义的操作

写法： **<! [ CDATA [ 内容 ]]>**

如

```xml
if(a<b && b<c && d<f){}
可以写成：
<!CDATA [<b> if (a<b && B<c && d<f){}</b> ]]>
```

把特殊字符当做文本内容，而不是标签

## XML约束

**约束的作用：**

如现在定义一个person的xml文件，只想要这个文件里面保存人的信息，比如name age等，但是如果在xml文件中写一个标签<猫>，发现可以正常显示，因为符合语法规范。但是猫肯定不是人的信息，xml的标签是自定义的，需要技术来规定xml中只能出现的元素，这个时候需要约束。

**xml的约束的技术：DTD约束 和Schema约束。**

### DTD约束

#### 1.dtd引入的三种方式

**A：使用内部的dtd文件，将约束规则定义在xml文档中**

```dtd
<!DOCTYPE 根元素名称 [
    	<!ELEMENT 	根元素名称 (person+)>
			<!ELEMENT 	person (name,age)>
			<!ELEMENT		name (#PCDATA)>
			<!ELEMENT 	age (#PCDATA)>
]>
```

**B：引入外部的dtd文件**

```dtd
<!DOCTYPE  根元素名称 SYSTEM "dtd路径">
```

C：使用外部的dtd文件（网络上的dtd文件）

```dtd
<!DOCTYPE 根元素 PUBLIC "DTD名称" "DTD文档的URL"

例如使用struts2框架 ，使用配置文件时所使用的外部的dtd文件
<!DOCTYPE struts PUBLIC   "-//Apache Software Foundation//DTD
Struts Configuration 2.0//EN"    
"http://struts.apache.org/dtds/struts-2.0.dtd">
```

#### 2.使用dtd定义元素

<!ELEMENT 元素名 约束>

**A：简单元素（没有子元素）**

```dtd
<!ELEMENT name (#PCDATA)>
 
 (#PCDATA)：约束，PCDATA是字符串类型
 	EMPTY：元素为空（没有内容）例如：<sex></sex>
 	ANY：任意
```

**B：复杂元素**

```xml-dtd
<!-- 语法 -->
<!ELEMENT person (id,name,age)> 子元素只能出现一次

<!ELEMENT 元素名称 (子元素)>

<!-- 子元素出现次数 -->
* ：一次多或多次
？：零次或一次
* ：零次或多次

<!-- 子元素直接使用逗号隔开 -->
	表示元素出现的顺序 

<!-- 子元素直接使用 | -->
	表示元素只能出现其中的任意一个
```

#### 3.使用dtd定义属性

```xml-dtd
<!-- 语法 -->
<!ATTLIST 元素名称 	属性名称 属性类型 属性约束>

<!-- 属性类型 -->  CDATA：字符串
<!ATTLIST birthday ID1 CDATA #REQUIRED>

<!-- 枚举 -->
表示只能在一定的范围内出现值，但是只能每次出现其中的一个,红绿灯效果
<!ATTLIST age	ID2 (AA|BB|CC)  #REQUIRED >

<!-- ID: 值只能是字母或者下划线开头 -->
<!ATTLIST name 	ID3 ID   #REQUIRED >

<!--属性的约束 -->
#REQUIRED:  属性必须存在
#IMPLIED:		属性可与可无
#FIXED:			表示一个固定值
			#FIXED "AAA" 属性的值必须是设置的这个固定值


直接值
不写属性，使用直接值
写了属性，使用设置那个值
<!ATTLIST sex			ID4 CDATA #FIXED "ABC"		>
<!ATTLIST school			ID5 CDATA "WWW"		>
```

### Schema约束

schema 符合 xml 的语法，一个 xml 中可以有多个 schema ，多个 schema 使用名称空间区分（类似于java包名）dtd 里面有PCDATA类型，但是在 schema 里面可以支持更多的数据类型

后缀名：xsd

```schema
引入：
填写xml文档的根元素

引入xsi前缀.  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	表示xml文件是一个约束文件
	
引入xsd文件命名空间.  xsi:schemaLocation="http://www.bwh.cn/xml  student.xsd"
	使用一个使用schema约束文件，直接通过这个地址引入约束文件
	  通常使用一个url地址防止重名
	  
为每一个xsd约束声明一个前缀,作为标识  xmlns="http://www.bwh.cn/xml" 
```

**(1) 看xml中有多少个元素**

```
<element>
```

**(2) 看简单元素和复杂元素**

```xml
<element name="person">
<complexType>
<sequence>
	<element name="name" type="string"></element>
					<element name="age" type="int"></element>
</sequence>
</complexType>
</element>
```

**(3) 被约束文件里面引入约束文件**

```xml
<person xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://www.bwh.cn/20151111"
xsi:schemaLocation="http://www.bwh.cn/20151111 1.xsd">

			xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
				-- 表示xml是一个被约束文件
			xmlns="http://www.bwh.cn/20151111"
				-- 是约束文档里面 targetNamespace
			xsi:schemaLocation="http://www.bwh.cn/20151111 1.xsd">
				-- targetNamespace 空格  约束文档的地址路径
```

**可以约束属性**

```xml
A: <sequence>：表示元素的出现的顺序
B: <all>: 元素只能出现一次
C: <choice>：元素只能出现其中的一个
D: maxOccurs="unbounded"： 表示元素的出现的次数
E: <any></any>:表示任意元素

写在复杂元素里面
写在　</complexType>之前

－－
<attribute name="id1" type="int" use="required"></attribute>
	- name: 属性名称
	- type：属性类型 int stirng
	- use：属性是否必须出现 required
```

## XML解析

### DOM解析

```java
package xml;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;
import javax.imageio.metadata.IIOMetadataNode;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.transform.*;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @author beifeng
 * @date 2020/8/21
 * 解析users xml
 */
public class DomReader {
    private Document document;
//    获取Document对象
    public  void getDocument(){
        try {
//            创建转换器工厂
            DocumentBuilderFactory bf=DocumentBuilderFactory.newInstance();
//            创建转换器
            DocumentBuilder db=  bf.newDocumentBuilder();
//            获取document对象 
            document=db.parse("src/xml/users.xml");
        } catch (ParserConfigurationException e) {
            e.printStackTrace();
        } catch (SAXException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
    public  void selectAll(){
            NodeList users=document.getElementsByTagName("users").item(0).getChildNodes();
            for (int i = 0; i <users.getLength() ; i++) {
                Node user=users.item(i);
                if (user.getNodeType() == Node.ELEMENT_NODE) {
                    Element useEle=(Element)user;
//                    user.getAttributes().item(0).getNodeValue()
                    System.out.println(user.getNodeName()+"\t"+useEle.getAttribute("id"));
                    NodeList shuxing=user.getChildNodes();
                        for (int j = 0; j < shuxing.getLength(); j++) {
                            if (shuxing.item(j).getNodeType() == Node.ELEMENT_NODE) {
                            String name = shuxing.item(j).getNodeName();
                            System.out.println(name + "=" + shuxing.item(j).getTextContent());
                            }
                         }
                }
                System.out.println();
            }
    }
//    修改元素节点
    public  void update() throws TransformerException, FileNotFoundException {
        NodeList upNode=document.getElementsByTagName("user");
        for (int i = 0; i <upNode.getLength() ; i++) {
//            System.out.println( upNode.item(i).getAttributes().item(0).getNodeValue());
            if (upNode.item(i).getNodeType() == Node.ELEMENT_NODE && upNode.item(i).getAttributes().item(0).getNodeValue().equals(i+"")) {
                Element upEle=(Element)upNode.item(i);
                upEle.setAttribute("id","A"+i);
            }
        }
        saveXml();
    }
//  保存xml文件
    public void  saveXml() throws TransformerException, FileNotFoundException {
        //创建转换器工厂
        TransformerFactory tff= TransformerFactory.newInstance();
        //创建转换器
        Transformer tf=tff.newTransformer();
        //设置编码格式
        tf.setOutputProperty(OutputKeys.ENCODING,"utf-8");
        tf.setOutputProperty(OutputKeys.INDENT,"YES");
        //创建DOMSource对象
        DOMSource source= new DOMSource(document);
        //建立输出流
        StreamResult result=new StreamResult(new FileOutputStream("src/xml/users.xml"));
        //将dom写入xml文件中
        tf.transform(source,result);

    }
    //删除节点
    public void delete() throws TransformerException, FileNotFoundException {
        Node sex=document.getElementsByTagName("sex").item(0);
//        根据父节点删除内容
        Node user=sex.getParentNode();
        user.removeChild(sex);
        System.out.println("删除成功！");
        saveXml();

    }
    //添加节点
    public void add() throws TransformerException, FileNotFoundException {
//        添加元素节点
        Element other=document.createElement("other");
//        添加属性节点
        other.setAttribute("name","skr");
//        添加文本节点
        other.setTextContent("otherContent");
        //根据父节点添加内容
        Node node=document.getElementsByTagName("user").item(0);
        node.appendChild(other);
        saveXml();
    }
    public static void main(String[] args) throws TransformerException, FileNotFoundException {
        DomReader domReader=new DomReader();
        domReader.getDocument();
        domReader.add();
//        domReader.delete();

//        domReader.update();
        domReader.selectAll();

    }

}
```

xml文件

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<users>
        <user id="0">
            <name>Mary</name>
            <age>23</age>
            <sex>Female</sex>
        </user>
        <user id="1">
            <name>Mike</name>
            <age>24</age>
            <sex>Female</sex>
        </user>
        <user id="2">
            <name>Alice</name>
            <age>23</age>
            <sex>Female</sex>
        </user>
        <user id="3">
            <name>Tom</name>
            <age>24</age>
            <sex>Male</sex>
        </user>
    </users>
```

### DOM4J解析

```java
package xml;

import org.dom4j.*;
import org.dom4j.io.OutputFormat;
import org.dom4j.io.SAXReader;
import org.dom4j.io.XMLWriter;

import java.io.*;
import java.util.Iterator;

/**
 * @author beifeng
 * @date 2020/8/25
 */
public class Dom4jDemo {
    private Document document;

    //获得document对象
    public void loadDocument() {
        SAXReader saxReader = new SAXReader();
        try {
            document = saxReader.read(new File("src/xml/phones.xml"));
        } catch (DocumentException e) {
            e.printStackTrace();
        }
    }

    //显示手机的品牌和型号
    
   /*  1、创建解析器
       2、得到document
       3、得到根节点  getRootElement() 返回Element
       4、节点.elementIterator 循环遍历xml文件
   */
    public void showPhoneInfo() {
//        获取根节点
        Element root = document.getRootElement();
//        遍历phone元素节点
        Iterator phoneIter = root.elementIterator();
        while (phoneIter.hasNext()) {
            Element phone = (Element) phoneIter.next();
            System.out.println(phone.attributeValue("name"));
            Iterator preIter = phone.elementIterator();
            while (preIter.hasNext()) {
                Element preEle = (Element) preIter.next();
                String message = preEle.attributeValue("name") != null ? preEle.attributeValue("name") : preEle.getText();
                System.out.println(preEle.getName() + ":\t" + message);
            }
        }
    }

    //保存xml文件
    public void saveXml(String path) {
        OutputFormat outputFormat = OutputFormat.createPrettyPrint();
        try {
            XMLWriter writer = new XMLWriter(new OutputStreamWriter(new FileOutputStream(path)),outputFormat);
            writer.write(document);
            writer.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public void addPhone() {
//        <Type name="M8"/>
//        <price>1000&lt;price&lt;5000</price>
        Element root = document.getRootElement();
        Element phone = root.addElement("phone");
        phone.addAttribute("name", "小米");
        Element type=phone.addElement("Type");
        type.addAttribute("name","M8");
        Element price=phone.addElement("price");
        price.addText("1000>price<5000");
        saveXml("src/xml/phones.xml");


    }
    public void update(){
        Element root=document.getRootElement();
        Iterator phone=root.elementIterator();
        while(phone.hasNext()){
            Element element=(Element)phone.next();
            if (element.attributeValue("name").equals("小米")) {
                element.addAttribute("name","xiaoMi");
                element.addAttribute("id","d");
            }
        }
        saveXml("src/xml/phones.xml");
    }
    public void delete(){
        Element root= document.getRootElement();
        Iterator phoneIter=root.elementIterator();
        while(phoneIter.hasNext()){
            Element element=(Element) phoneIter.next();
            if(element.attributeValue("name").equals("小米")){
//                element.getParent().remove(element);
                Iterator  iter=element.elementIterator();
                while(iter.hasNext()){
                    Element attri=(Element)iter.next();
                    if (attri.getName().equals("price")) {
                        attri.getParent().remove(attri);
                    }

                }
            }
        }
        saveXml("src/xml/phones.xml");
    }
    public static void main(String[] args) {
        Dom4jDemo demo = new Dom4jDemo();
        demo.loadDocument();
        demo.update();
//        demo.delete();
//        demo.addPhone();
        demo.showPhoneInfo();
    }
}

```

xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--phones 手机-->
<phones> 
  <phone name="华为"> 
    <Type name="U8650"/>  
    <price>price&gt;3000</price> 
  </phone>  
  <phone name="苹果"> 
    <Type name="iphone7"/>  
    <price>1000&lt;price&lt;5000</price> 
  </phone>  
  <phone name="xiaoMi" id="d"> 
    <Type name="M8"/> 
  </phone>  
  <phone name="xiaoMi" id="d"> 
    <Type name="M8"/> 
  </phone> 
</phones>

```

