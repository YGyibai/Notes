# Java文件读写

## java文件基本操作

### 文件类File

**常用方法：**

| 方法名                        | 含义                                                       |
| ----------------------------- | ---------------------------------------------------------- |
| boolean   creatNewFile        | 创建文件                                                   |
| boolean   exists              | 判断文件是否存在                                           |
| String        getAbsolutePath | 获取文件的绝对路径                                         |
| String        getName         | 获取文件名                                                 |
| String        getParent       | 返回此File对象的上一级目录，如果没有上一级目录，则返回null |
| String        getPath         | 获取文件的路径                                             |
| String       lastModified     | 获取文件最后的修改时间                                     |
| boolean   mkdir               | 创建一个目录，它的路径由当前的File对象指定                 |
| boolean  mkdirs               | 创建包括父目录的目录                                       |

**示例**

```java
  File file=new File("/Users/beifeng/temp");
        if (!file.exists()){
            file.mkdirs();//mkdir 创建单级目录， mikdirs 创建多级目录
        }
        //isDirectory 检测file是否是目录
        System.out.println("Is file directory\t"+file.isDirectory());
        //创建文件
        File f=new File("/Users/beifeng/temp/abc.txt");
        if (!f.exists()){
            try {
                f.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();//可能因为权限不足或磁盘已满报错
            }
        }
        //输出文件相关属性
        System.out.println("Is f file?\t"+f.isFile());//是否是文件
        System.out.println("name\t"+f.getName());//返回文件名字
        System.out.println("parent\t"+f.getParent());//返回file文件的上级目录
        System.out.println("Path\t"+f.getPath());//返回文件或目录的路径
        System.out.println("Size\t"+f.length()+"bytes");//获取文件或目录的大小
        System.out.println("Last modified time\t"+f.lastModified());//获取文件的最后修改日期
        //遍历temp目录下的所有的文件信息
        System.out.println("list files in file directory");
        File[] fs=file.listFiles();//列出file目录下所有的子文件，但不包括子文件下的目录
        if (fs!=null){
            for(File f1:fs){
                System.out.println(f1.getPath()
                );
            }
        }
//        f.delete();
//        file.delete();
```

### Java7提出的NIO包，提出新的文件系统类

Path，Files，DirectoryStream。FileVisitor，FileSystem

是java.io.File的有益补充。

主要功能有：		文件复制和移动		文件相对路径		递归遍历目录		递归删除目录

**Path类**

```java
  //Path和java.io.File基本类似
        //获得path方法一，
        Path path= FileSystems.getDefault().getPath("/User/beifeng","abc.txt");
        System.out.println(path.getFileName());
        //获得path方法二，用File的toPath（）方法获得path对象
        File file=new File("/User/beifeng/abc.txt");
        Path pathOther=file.toPath();
        System.out.println(path.compareTo(pathOther)); //0，说明这两个path是相等的
        //获得Path方法三
        Path path3= Paths.get("/User/beifeng","abc.txt");
        System.out.println(path3.toString());
```

**Files类**

```java

	public static void moveFile() {
		Path from = Paths.get("c:/temp", "abc.txt");
		//移动c:/temp/abc.txt到c:/temp/test/def.txt，如目标文件已存在，就替换
		Path to = from.getParent().resolve("test/def.txt");
		try {
			//文件的大小bytes
			System.out.println(Files.size(from));
			//调用文件移动方法  如果目标文件已经存在，就替换
			Files.move(from, to, StandardCopyOption.REPLACE_EXISTING);
		} catch (IOException e) {
			System.err.println("移动文件错误" + e.getMessage());
		}
	}
```

## Java  IO包

​		java读写文件，只能以**（数据）流**的形式进行读写

### Java IO包分为三类

#### 		-节点类：直接对文件进行操作

​				InputStream ,	OutputStream(字节)

​					 		子类： FileInputStream，	FileOutputStram

​				Reader,Write(字符)

​							子类：FileReader，FileWriter

#### 		-转化类：字节/字符/数据类型的转化类

​					**字符到字节之间的转化**

​					InputStreamReader：文件读取字节，转化为java能理解的字符

​					OutputStreamWriter：Java将字符转化为字节输入到文件中

#### 		-装饰类：装饰节点类

​					DataInputStream，DataOutputStream：封装数据流

​					BufferedInputStream，BufferOuputStream：缓存字节流

​					BufferedReader，BufferedWriter：缓存字符流

![IO流](/Users/beifeng/Documents/IO流.png)

## 文本文件读写

### 写文件：

​		先创建文件，写入数据，关闭文件。

​		主要用到的类：	FileOutputStream，OutputStreamWriter，

​										BufferedWriter，	（write，newLine）

​		try-resource语句，自动关闭资源。

​		关闭最外层的数据流，将会把 其上的所有数据流都关闭

```java
    public static void writeFile(){
        FileOutputStream fos=null;
        OutputStreamWriter osw=null;
        BufferedWriter bw=null;
        try {
            fos=new FileOutputStream("/Users/beifeng/temp/abc.txt");//找到文件，获取文件节点
            osw=new OutputStreamWriter(fos);//字符和字节流的转换区
            bw=new BufferedWriter(osw);//创建字符缓冲区
            bw.write("我们是");
            bw.newLine();
            bw.write("Ecnume^^");
            bw.newLine();
        } catch (IOException e) {
            e.printStackTrace();
        } finally{
            try {
                bw.close();
            }catch(Exception ex){
                ex.printStackTrace();
            }
        }

    }
		
    public static void write(){
        //try-resource语句，自动关闭资源
        try(BufferedWriter bw=new BufferedWriter(new OutputStreamWriter(new FileOutputStream("/Users/beifeng/temp/abc.txt")))){
                bw.write("welcome");
                bw.newLine();
                bw.write("go home");
                bw.newLine();
        }catch(Exception e){
            e.printStackTrace();
        }

    }
```



### 读文件：

​		先打开文件，逐行读入数据，关闭文件。

​		主要用到的类：	FileInputStream，InputStreamReader，

​										BufferedReader，	(write，newLine）

​		try-resource语句，自动关闭资源。

​		关闭最外层的数据流，将会把 其上的所有数据流都关闭

```java
public static void reader(){
        FileInputStream fis=null;
        InputStreamReader isr=null;
        BufferedReader br=null;
        try {
            fis = new FileInputStream("/Users/beifeng/temp/abc.txt");
            isr=new InputStreamReader(fis);
            br=new BufferedReader(isr);
            String line;
            while((line=br.readLine())!=null){
                System.out.println(line);
            }
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            try {
                br.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
    public static void readerFile(){
        try(BufferedReader br=new BufferedReader(new InputStreamReader(new FileInputStream("/Users/beifeng/temp/abc.txt")))){
                String line;
                while((line=br.readLine())!=null){
                    System.out.println(line);
                }
        }
        catch (Exception e){
            e.printStackTrace();
        }
    }
```

### 整理

**字节流和字符流区别：**

​		字符流在操作时使用了缓冲区，而字符流在操作时直接操作文件，不会使用缓冲区

```
/**
 *InputStream、OutputStream、Reader、Writer里都有read方法
 *InputStream OutputStream 都是byte数组
 *Reader是char数组		Writer是String类型
 *
 * read() :  从输入流中读取数据的下一个字节，返回0到255范围内的int字节值。
 * 如果因为已经到达流末尾而没有可用的字节，则返回-1。在输入数据可用、检测到流末尾或者抛出异常前，
 * 此方法一直阻塞。
 *
 * read(byte[] b) :  从输入流中读取一定数量的字节，并将其存储在缓冲区数组 b 中。
 * 以整数形式返回实际读取的字节数。在输入数据可用、检测到文件末尾或者抛出异常前，此方法一直阻塞。
 *
 * 如果 b 的长度为 0，则不读取任何字节并返回 0；否则，尝试读取至少一个字节。
 * 如果因为流位于文件末尾而没有可用的字节，则返回值 -1；否则，至少读取一个字节并将其存储在 b 中。
 *
 * read(byte[] b,int off, int length) 从输入流中读取数据，保存到数组b中，保存的位置从off开始
 */
```

### 读写二进制文件

写二进制文件

```java
 public static void write(){
        try {
            FileOutputStream fis = new FileOutputStream("/Users/beifeng/temp/jkl.txt");
            BufferedOutputStream bw=new BufferedOutputStream(fis);
            DataOutputStream dis = new DataOutputStream(bw);
            dis.writeByte(1);
            dis.writeLong(2);
            dis.writeChar('a');
            dis.writeUTF("hello");
            dis.flush();
            dis.close();

        }catch(IOException e){
            e.printStackTrace();
        }
    }
```

读二进制文件

```java
public static void read(){
        try {
            FileInputStream fs = new FileInputStream("/Users/beifeng/temp/jkl.txt");
            BufferedInputStream bis = new BufferedInputStream(fs);
            DataInputStream dis = new DataInputStream(bis);
            System.out.println(dis.readByte());
            System.out.println(dis.readLong());
            System.out.println(dis.readChar());
            System.out.println(dis.readUTF());
            dis.close();

        }catch(IOException e){
            e.printStackTrace();
        }
    }
```

### 重定向标准	I/O

​	**System.in 常见方法如下：**

​		int read() ，此方法从接盘接受一个字节的数据，返回值是该字符的ASCII码

​		int read(byte[] buf)，此方法从键盘接受多个字节的数据，保存至buf中，返回值是接受字节数据的个数，非ASCII码

​		int read(byte[] buf,int off,int length) 和第二个方法返回值一样，off是开始的位置

​	**System.out 常见方法如下：**

​		print（）

​		println()

System类提供了三个重定向标准输入/输出的方法

| 方法                                               | 说明                 |
| -------------------------------------------------- | -------------------- |
| static     void    setErro( PrintStream       err) | 重定向标准错误输出流 |
| static     void    setIn(  InputStream        in)  | 重定向标准输入流     |
| static     void    setOut(  OutputStream   out)    | 重定向标准输出流     |

**setOut**

```java
public static void setO(){
        try {
            PrintStream ps = new PrintStream(new FileOutputStream("/Users/beifeng/temp/abc.txt"));
            System.setOut(ps);
            System.out.println("我的测试，重定向到文件abc");
        }catch(IOException e){
            e.printStackTrace();
        }
    }
```

**setIn**

```java
public static void  getI(){
        try {
            System.setIn(new FileInputStream("/Users/beifeng/temp/abc.txt"));
            byte[] bs=new byte[System.in.available()];
            while(  System.in.read(bs,0,bs.length)!=-1){
                System.out.println(new String(bs));
            }
        }catch(IOException e){
            e.printStackTrace();
        }
    }
```

