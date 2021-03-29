---
title: I/O
date: 2021-03-17 15:30:43
tags:
- Java
categories:
- Java
---

## 字符集/编码集

字符集：各个国家为自己国家的字符取的一套编号规则。

1B = 8b   计算机中的最小单位是字节B.

英文和数字在任何编码集中都是一样的，都占1个字节。

GBK编码中，1个中文字符一般占2个字节。

UTF-8编码中，1个中文字符一般占3个字节。

## IO流读写数据

IO输入输出流：输入/输出流。Input:输入。Output:输出。

File类只能操作文件对象本身，不能读写文件对象的内容。读写数据内容，应该使用IO流。

### IO流的分类

按照流的方向分为：输入流，输出流。

1. 输出流：以内存为基准，把内存中的数据写出到磁盘文件或者网络介质中去的流称为输出流。输出流的作用：写数据到文件，或者写数据发送给别人。

2. 输入流：以内存为基准，把磁盘文件中的数据或者网络中的数据读入到内存中去的流称为输入流。输入流的作用：读取数据到内存。

按照流的内容分为: 字节流，字符流。

1. 字节流：流中的数据的最小单位是一个一个的字节，这个流就是字节流。
2. 字符流：流中的数据的最小单位是一个一个的字符，这个流就是字符流。(针对于文本内容)

所以流大体分为四大类:

1. 字节输入流：以内存为基准，把磁盘文件中的数据或者网络中的数据以一个一个的字节的形式读入到内存中去的流称为字节输入流。
2. 字节输出流：以内存为基准，把内存中的数据以一个一个的字节写出到磁盘文件或者网络介质中去的流称为字节输出流。
3. 字符输入流：以内存为基准，把磁盘文件中的数据或者网络中的数据以一个一个的字符的形式读入到内存中去的流称为字符输入流。
4. 字符输出流：以内存为基准，把内存中的数据以一个一个的字符写出到磁盘文件或者网络介质中去的流称为字符输出流。

IO流的体系：

|                | 字节流             | 字节流               | 字符流       | 字符流 |
| ---------------| ----------------- | ------------------ | ------------ | ---------- |
|                | 字节输入流          | 字节输出流           | 字符输入流     | 字符输出流 |
| 抽象类          | `InputStream`     | `OutputStream`     | `Reader`     | `Writer`        |
| 子类实现类       | `FileInputStream` | `FileOutputStream` | `FileReader` | `FileWriter `  |

### 字节输入流的使用

`FileInputStream`文件字节输入流。作用：以内存为基准，把磁盘文件中的数据按照字节的形式读入到内存中的流。

简单来说，就是按照字节读取文件数据到内存。

构造器：
1. `public FileInputStream(File path)`:创建一个字节输入流管道与源文件对象接通。
2. `public FileInputStream(String pathName)`:创建一个字节输入流管道与文件路径对接。

#### 按照字节读取

方法：`public int read()`:每次读取一个字节返回！读取完毕会返回-1。

一个一个字节读取英文和数字没有问题。但是一旦读取中文输出无法避免乱码，因为会截断中文的字节。一个一个字节的读取数据，性能也较差，所以禁止使用此方案！

```java
// 1.创建文件对象定位dlei01.txt
File file = new File("Day09Demo/src/dlei01.txt");
// 2.创建一个字节输入流管道与源文件接通
InputStream is = new FileInputStream(file);

// 3.使用while读取字节数
// 读取没有字节返回-1
// 定义一个整数变量存储字节
int ch = 0 ;
while((ch = is.read())!= -1){
    System.out.print((char) ch);
}
```

#### 按照字节数组读取

方法：`public int read(byte[] buffer)`从字节输入流中读取字节到字节数组中去，返回读取的字节数量，没有字节可读返回-1。

```java
// 简化写法：直接创建一个字节输入流管道与源文件路径接通。
InputStream is = new FileInputStream("Day09Demo/src/dlei02.txt");
```

```java
// 文件内容：abcxyzi
// 4.定义一个字节数组读取数据（定义一个桶）
byte[] buffer = new byte[3];
// 从is管道中读取字节装入到字节数组中去，返回读取字节的数量。
int len = is.read(buffer);
System.out.println("读取了字节数："+len);
String rs = new String(buffer);
System.out.println(rs); // abc

int len1 = is.read(buffer);
System.out.println("读取了字节数："+len1);
String rs1 = new String(buffer);
System.out.println(rs1); // xyz

int len2 = is.read(buffer);
System.out.println("读取了字节数："+len2);
// 倒出字节数组中的全部字符
//String rs2 = new String(buffer); // iyz
// 读取了多少就倒出多少！
String rs2 = new String(buffer, 0 , len2); // i
System.out.println(rs2); 
```

```java
// 读法优化，必须使用循环     // abc xyz i
// a.定义一个字节数组代表桶   // ooo ooo o
byte[] buffer = new byte[1024];
int len ; // 存储每次读取的字节数。
while((len = is.read(buffer)) != -1){
    // 读取了多少就倒出多少！
    String rs = new String(buffer , 0 , len);
    System.out.print(rs);
}
```

 使用字节数组读取内容，效率可以。但是使用字节数组读取文本内容输出，也无法避免中文读取输出乱码的问题。

---

解决字节输入流读取中文内容输出乱码的问题。

一个一个字节读取中文输出、一个一个字节数组读取中文输出均无法避免乱码。

定义一个字节数组与文件的大小刚刚一样大，然后一桶水读取全部字节数据再输出！可以避免中文读取输出乱码。

但是如果读取的文件过大，会出现内存溢出！

```java
// 0.定位文件对象
File f = new File("Day09Demo/src/dlei03.txt");
// 1.定义一个字节输入流通向源文件路径
InputStream is = new FileInputStream(f);
// 2.定义一个字节数组与文件的大小刚刚一样大
System.out.println("文件大小："+f.length());
byte[] buffer = new byte[(int) f.length()];
int len = is.read(buffer);
System.out.println("读取了："+len);
String rs = new String(buffer);
System.out.println(rs);
```

```java
// 2.API方法 JDK1.9
byte[] buffer = is.readAllBytes();
String rs = new String(buffer);
System.out.println(rs);
```

字节流并不适合读取文本文件内容输出，读写文件内容建议使用字符流。

### 字节输出流的使用

作用：以内存为基准，把内存中的数据，按照字节的形式写出到磁盘文件中去。简单来说，把内存数据按照字节写出到磁盘文件中去。

构造器：

1. `public FileOutputStream(File file)`:创建一个字节输出流管道通向目标文件对象。
2. `public FileOutputStream(String file)`:创建一个字节输出流管道通向目标文件路径。
3. `public FileOutputStream(File file , boolean append)`:创建一个追加数据的字节输出流管道通向目标文件对象。
4. `public FileOutputStream(String file , boolean append)`:创建一个追加数据的字节输出流管道通向目标文件路径。字节输出流默认是覆盖数据管道。`append=true`追加数据

方法：

1. `public void write(int a)`:写一个字节出去 。
2. `public void write(byte[] buffer)`:写一个字节数组出去。
3. `public void write(byte[] buffer , int pos , int len)`:写一个字节数组的一部分出去。参数一，字节数组；参数二：起始字节索引位置，参数三：写多少个字节数出去。

```java
// 1. 创建一个文件对象定位目标文件(写数据到的那个文件，会自动创建)
File file = new File("Day09Demo/src/dlei04.txt");
// 2. 创建字节输出流管道与目标文件对象接通
FileOutputStream os = new FileOutputStream(file);
// 3. 写数据
// a. 写一个字节出去(中文只写第一各字节)
os.write(97);
os.write('b');

// 刷新数据到文件中去
os.flush();

// 换行用
os.write("\r\n".getBytes());

// b. 写一个字节数组出去
byte[] bytes = new byte[]{48,49,53,54};
os.write(bytes);
byte[] bytes1 = "Halo，世界".getBytes();
os.write(bytes1);

os.write("\r\n".getBytes());

// c. 写一个字节数组的一部分出去。
byte[] bytes2 = "Halo，世界".getBytes();
os.write(bytes2, 0, 4);

// 关闭资源管道(关闭包含了刷新)
os.close();
```

### 字节流做文件复制

字节是计算机中一切文件的组成，所以字节流适合做一切文件的复制。

复制是把源文件的全部字节一字不漏的转移到目标文件，只要文件前后的格式一样，绝对不会有问题。

分析步骤：

1. 创建一个字节输入流管道与源文件接通。
2. 创建一个字节输出流与目标文件接通。
3. 创建一个字节数组作为桶
4. 从字节输入流管道中读取数据，写出到字节输出流管道即可。
5. 关闭资源！

```java
public static void main(String[] args) {
    InputStream is = null ;
    OutputStream os = null ;
    try{
        // 1. 创建一个字节输入流管道与源文件接通。
        is = new FileInputStream("D:\\itcast\\图片资源\\meinv.jpg");
        // 2. 创建一个字节输出流与目标文件接通。
        os = new FileOutputStream("D:\\itcast\\meimei.jpg");
        // 3. 创建一个字节数组作为桶
        byte[] buffer = new byte[1024];
        // 4. 从字节输入流管道中读取数据，写出到字节输出流管道即可。
        int len = 0;
        while((len = is.read(buffer)) != -1){
            // 读取多少就倒出多少
            os.write(buffer, 0 , len);
        }
        System.out.println("复制完成！");
    }catch (Exception e){
        e.printStackTrace();
    } finally {
        // 5. 关闭资源(最好分别try-catch)
        try{
            if(os!=null)os.close();
            if(is!=null)is.close();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

---

JDK 1.7 资源释放的新方式

```java
try-with-resources:
try(
    // 这里只能放置资源对象，用完会自动调用close()关闭
){

}catch(Exception e){
    e.printStackTrace();
}
```

```java
try(
    InputStream is  = new FileInputStream("D:\\itcast\\图片资源\\meinv.jpg");
    OutputStream os = new FileOutputStream("D:\\itcast\\meimei.jpg");
){
    byte[] buffer = new byte[1024];
    int len = 0;
    while((len = is.read(buffer)) != -1){
        os.write(buffer, 0 , len);
    }
    System.out.println("复制完成！");
}catch (Exception e){
    e.printStackTrace();
}
```

资源类一定是实现了`Closeable`接口，实现这个接口的类就是资源

 有`close()`方法，try-with-resources会自动调用它的`close()`关闭资源。

### 字符输入流的使用

`FileReader`文件字符输入流：以内存为基准，把磁盘文件的数据以字符的形式读入到内存。简单来说，读取文本文件内容到内存中去。

 构造器：

1. `public FileReader(File file)`:创建一个字符输入流与源文件对象接通。
2. `public FileReader(String filePath)`:创建一个字符输入流与源文件路径接通。

方法：

1. `public int read()`: 读取一个字符的编号返回！ 读取完毕返回-1
2. `public int read(char[] buffer)`:读取一个字符数组，读取多少个字符就返回多少个数量，读取完毕返回-1

#### 按照字符读取

字符流一个一个字符的读取文本内容输出，可以解决中文读取输出乱码的问题。字符流很适合操作文本文件内容。

但是：一个一个字符的读取文本内容性能较差！！

```java
Reader fr = new FileReader("Day10Demo/src/dlei01.txt");
int ch ;
while ((ch = fr.read()) != -1){
    System.out.print((char)ch);
}
```

#### 按照字符数组读取

字符流按照字符数组循环读取数据，可以解决中文读取输出乱码的问题，而且性能也较好！！

```java
Reader fr = new FileReader("Day10Demo/src/dlei02.txt");
char[] buffer = new char[1024]; // 1K
// b.定义一个整数记录每次桶读取的字符数据量。
int len;
while((len = fr.read(buffer)) != -1 ) {
    // 读取多少倒出多少字符
    System.out.print(new String(buffer, 0 , len));
}
```

### 字符输出流的使用

`FileWriter`文件字符输出流，以内存为基准，把内存中的数据按照字符的形式写出到磁盘文件中去。简单来说，就是把内存的数据以字符写出到文件中去。

构造器：

1. `public FileWriter(File file)`:创建一个字符输出流管道通向目标文件对象。
2. `public FileWriter(String filePath)`:创建一个字符输出流管道通向目标文件路径。
3. `public FileWriter(File file,boolean append)`:创建一个追加数据的字符输出流管道通向目标文件对象。
4. `public FileWriter(String filePath,boolean append)`:创建一个追加数据的字符输出流管道通向目标文件路径。

方法：

1. `public void write(int c)`：写一个字符出去
2. `public void write(String c)`：写一个字符串出去
3. `public void write(char[] buffer)`:写一个字符数组出去
4. `public void write(String c ,int pos ,int len)`:写字符串的一部分出去
5. `public void write(char[] buffer ,int pos ,int len)`:写字符数组的一部分出去

```java
// 1.创建一个字符输出流管道通向目标文件路径
//Writer fw = new FileWriter("Day10Demo/src/dlei03.txt"); // 覆盖数据管道
Writer fw = new FileWriter("Day10Demo/src/dlei03.txt",true); // 追加数据管道

// 2.写一个字符出去：public void write(int c):写一个字符出去
fw.write(97);   // 字符a
fw.write('b');  // 字符b
fw.write('磊'); // 字符磊，此时没有任何问题。
fw.write("\r\n"); // 换行

// 3.写一个字符串出去：public void write(String c)写一个字符串出去：
fw.write("Java是最优美的语言！");
fw.write("我们在黑马学习它！");
fw.write("\r\n"); // 换行

// 4.写一个字符数组出去：public void write(char[] buffer):写一个字符数组出去
fw.write("我爱中国".toCharArray());
fw.write("\r\n"); // 换行

// 5.写字符串的一部分出去: public void write(String c ,int pos ,int len):写字符串的一部分出去
fw.write("Java是最优美的语言！",0,9);
fw.write("\r\n"); // 换行

// 6.写字符数组的一部分出去：public void write(char[] buffer ,int pos ,int len):写字符数组的一部分出去
fw.write("我爱中国".toCharArray(),0 ,2);
fw.write("\r\n"); // 换行

fw.close();
```

## 缓冲流

### 缓冲流的概述和分类

什么是缓冲流：缓冲流可以提高字节流和字符流的读写数据的性能。

|                | 字节流             | 字节流               | 字符流       | 字符流 |
| ---------------| ----------------- | ------------------ | ------------ | ---------- |
|                | 字节输入流          | 字节输出流           | 字符输入流     | 字符输出流 |
| 抽象类          | `InputStream`     | `OutputStream`     | `Reader`     | `Writer`        |
| 实现类，低级流，原始流 | `FileInputStream` | `FileOutputStream` | `FileReader` | `FileWriter `  |
| 实现类，缓冲流 | `BufferedInputStream` | `BufferedOutputStream` | `BufferedReader` | `BufferedWriter` |

缓冲流分为四类：

1. `BufferedInputStream`：字节缓冲输入流，可以提高字节输入流读数据的性能。
2. `BufferedOutStream`：  字节缓冲输出流，可以提高字节输出流写数据的性能。
3. `BufferedReader`：  字符缓冲输入流，可以提高字符输入流读数据的性能。
4. `BufferedWriter`：  字符缓冲输出流，可以提高字符输出流写数据的性能。



### 字节缓冲输入流的使用

`BufferedInputStream`可以把低级的字节输入流包装成一个高级的缓冲字节输入流管道，从而提高字节输入流读数据的性能。

构造器: `public BufferedInputStream(InputStream in)`

```java
// 定义一个低级的字节输入流与源文件接通
InputStream is = new FileInputStream("Day10Demo/src/dlei04.txt");

// 把低级的字节输入流包装成一个高级的缓冲字节输入流。
BufferedInputStream bis = new BufferedInputStream(is);

// 定义一个字节数组按照循环读取。
byte[] buffer = new byte[3];
int len ;
while((len = bis.read(buffer)) != -1){
    String rs = new String(buffer, 0 , len);
    System.out.print(rs);
}
```

原理：缓冲字节输入流管道自带了一个8KB的缓冲池，每次可以直接借用操作系统的功能最多提取8KB的数据到缓冲池中去，以后我们直接从缓冲池读取数据，所以性能较好！

### 字节缓冲输出流的使用

`BufferedOutputStream`可以把低级的字节输出流包装成一个高级的缓冲字节输出流，从而提高写数据的性能。

构造器：`public BufferedOutputStream(OutputStream os)`

原理：缓冲字节输出流自带了8KB缓冲池,数据就直接写入到缓冲池中去，性能极高了！

字节缓冲输出流可以把低级的字节输出流包装成一个高级的缓冲字节输出流，从而提高写数据的性能。功能几乎不变。

```java
// 写一个原始的字节输出流
OutputStream os = new FileOutputStream("Day10Demo/src/dlei05.txt");
// 把低级的字节输出流包装成一个高级的缓冲字节输出流
BufferedOutputStream bos =  new BufferedOutputStream(os);
// 写数据出去
bos.write('a');
bos.write(100);
bos.write('b');
bos.write("我爱中国".getBytes());
bos.close();
```

### 字节缓冲流的性能统计分析

利用字节流的复制统计各种写法形式下缓冲流的性能执行情况。

复制流：

1. 使用低级的字节流按照一个一个字节的形式复制文件。
2. 使用低级的字节流按照一个一个字节数组的形式复制文件。
3. 使用高级的缓冲字节流按照一个一个字节的形式复制文件。
4. 使用高级的缓冲字节流按照一个一个字节数组的形式复制文件。



```java
public static final String SRC_FILE = "D:\\itcast\\班级\\Java就业128期\\day08\\15.并发包ConcurrentHashMap.avi";
public static final String DEST_FIlE = "D:\\itcast\\班级\\";
```

```java
/** （1）使用低级的字节流按照一个一个字节的形式复制文件。*/
public static void copy01(){
    long startTimer = System.currentTimeMillis();
    try(
        // 1.创建一个低级的字节输入流与源文件接通
        InputStream is = new FileInputStream(SRC_FILE);
        // 2.创建一个低级的字节输出流管道与目标文件接通
        OutputStream os = new FileOutputStream(DEST_FIlE+"01.avi");
    ){
        // 3.定义一个整型变量存储读取的字节。
        int ch ;
        while((ch = is.read())!=-1){
            os.write(ch);
        }
    }catch (Exception e){
        e.printStackTrace();
    }
    long endTimer = System.currentTimeMillis();
    System.out.println("低级的字节流按照一个一个字节的形式复制文件耗时："+(endTimer-startTimer)/1000.0);
}
```

```java
/**（2）使用低级的字节流按照一个一个字节数组的形式复制文件。*/
public static void copy02() {
    long startTimer = System.currentTimeMillis();
    try (
        InputStream is = new FileInputStream(SRC_FILE);
        OutputStream os = new FileOutputStream(DEST_FIlE + "02.avi");
    ) {
        byte[] buffer = new byte[1024];
        int len;
        while ((len = is.read(buffer)) != -1) {
            os.write(buffer, 0, len);
        }
    }
    catch (Exception e) {
        e.printStackTrace();
    }
    long endTimer = System.currentTimeMillis();
    System.out.println("低级的字节流按照一个一个字节数组的形式复制文件耗时：" + (endTimer - startTimer) / 1000.0);
}
```

```java
/**（3）使用高级的缓冲字节流按照一个一个字节的形式复制文件。*/
public static void copy03() {
    long startTimer = System.currentTimeMillis();
    try (
        InputStream is = new FileInputStream(SRC_FILE);
        BufferedInputStream bis = new BufferedInputStream(is);
        OutputStream os = new FileOutputStream(DEST_FIlE + "03.avi");
        BufferedOutputStream bos = new BufferedOutputStream(os);
    ) {
        int ch;
        while ((ch = bis.read()) != -1) {
            bos.write(ch);
        }
    }
    catch (Exception e) {
        e.printStackTrace();
    }
    long endTimer = System.currentTimeMillis();
    System.out.println("高级的缓冲字节流按照一个一个字节的形式复制文件耗时：" + (endTimer - startTimer) / 1000.0);
}
```

```java
/**(4)使用高级的缓冲字节流按照一个一个字节数组的形式复制文件。
     */
public static void copy04() {
    long startTimer = System.currentTimeMillis();
    try (
        InputStream is = new FileInputStream(SRC_FILE);
        BufferedInputStream bis = new BufferedInputStream(is);
        OutputStream os = new FileOutputStream(DEST_FIlE + "04.avi");
        BufferedOutputStream bos = new BufferedOutputStream(os);
    ) {
        byte[] buffer = new byte[1024];
        int len;
        while ((len = bis.read(buffer)) != -1) {
            bos.write(buffer, 0, len);
        }
    }
    catch (Exception e) {
        e.printStackTrace();
    }
    long endTimer = System.currentTimeMillis();
    System.out.println("高级的字节缓冲流按照一个一个字节数组的形式复制文件耗时：" + (endTimer - startTimer) / 1000.0);
}
```

```java
public static void main(String[] args) {
    // copy01(); // 低级流一个一个字节复制，速度太慢，简直让人无法忍受，直接淘汰，禁止使用！
    copy02(); // 低级的字节流按照一个一个字节数组的形式复制 ,读取较慢。5.264s
    copy03(); // 高级的缓冲字节流按照一个一个字节的形式复制 ,读取较慢。4.032s
    copy04(); // 高级的字节缓冲流按照一个一个字节数组的形式复制,速度极快。建议使用 0.71s
}
```

### 字符缓冲输入流的使用

`BufferedReader`字符缓冲输入流可以把字符输入流包装成一个高级的缓冲字符输入流，可以提高字符输入流读数据的性能。

构造器：`public BufferedReader(Reader reader):`

原理：缓冲字符输入流默认会有一个8K的字符缓冲池,可以提高读字符的性能。

缓冲字符输入流除了提高了字符输入流的读数据性能，缓冲字符输入流还多了一个按照行读取数据的功能（重点）:

 `public String readLine()`: 读取一行数据返回，读取完毕返回null;

```java
// 定义一个原始的字符输入流读取源文件
Reader fr = new FileReader("Day10Demo/src/dlei06.txt");

// 把低级的字符输入流管道包装成一个高级的缓冲字符输入流管道
BufferedReader br = new BufferedReader(fr);
// 定义一个字符串变量存储每行数据
String line;
// 使用一个循环读取数据(经典代码)
while((line = br.readLine())!=null){
    System.out.println(line);
}
br.close();
```

### 字符缓冲输出流的使用

`BufferedWriter`把字符输出流包装成一个高级的缓冲字符输出流，提高写字符数据的性能。

构造器：`public BufferedWriter(Writer writer):`

原理：高级的字符缓冲输出流多了一个8k的字符缓冲池，写数据性能极大提高了!

字符缓冲输出流除了提高字符输出流写数据的性能，还多了一个换行的特有功能:  `public void newLine()`新建一行。

```java
// 1.定义一个低级的字符输出流写数据出去
Writer fw = new FileWriter("Day10Demo/src/dlei07.txt",true);

// 3.把低级的字符输出流包装成高级的缓冲字符输出流
BufferedWriter bw = new BufferedWriter(fw);

// 2.写字符输出
bw.write("我在黑马学IO流~~~~");
bw.newLine(); // 换行
bw.write("我在黑马学IO流~~~~");
bw.newLine();// 换行

bw.close();
```

## 转换流

### 字符输入转换流的使用

`InputStreamReader`

|                | 字节流             | 字节流               | 字符流       | 字符流 |
| ---------------| ----------------- | ------------------ | ------------ | ---------- |
|                | 字节输入流          | 字节输出流           | 字符输入流     | 字符输出流 |
| 抽象类          | `InputStream`     | `OutputStream`     | `Reader`     | `Writer`        |
| 实现类，低级流，原始流 | `FileInputStream` | `FileOutputStream` | `FileReader` | `FileWriter `  |
| 实现类，缓冲流 | `BufferedInputStream` | `BufferedOutputStream` | `BufferedReader` | `BufferedWriter` |
|  |  |  | `InputStreamReader` | `OutputStreamWriter` |

`InputStreamReader`: 可以解决字符流读取不同编码乱码的问题。

2. 把原始的字节流按照当前默认的代码编码转换成字符输入流。
3. 把原始的字节流按照指定编码转换成字符输入流

构造器：

1. `public InputStreamReader(InputStream is)`：可以使用当前代码默认编码转换成字符流，几乎不用！
2. `public InputStreamReader(InputStream is,String charset)`:可以指定编码把字节流转换成字符流

```java
// 1.提取GBK文件的原始字节流
InputStream is = new FileInputStream("D:\\itcast\\网络编程公开课\\Netty.txt");
// 2.把原始字节输入流通过转换流，转换成 字符输入转换流InputStreamReader
//Reader isr = new InputStreamReader(is); // 使用当前代码默认编码UTF-8转换成字符流，几乎不用！
Reader isr = new InputStreamReader(is,"GBK"); // 指定编码把字节流转换成字符流
// 3.包装成缓冲流
BufferedReader br = new BufferedReader(isr);
// 4.定义一个字符串变量存储每行数据
String line;
// 使用一个循环读取数据(经典代码)
while((line = br.readLine())!=null){
    System.out.println(line);
}
```

### 字符输出转换流

`OutputStreamWriter`：可以指定编码把字节输出流转换成字符输出流。 即可以指定写出去的字符的编码。

构造器：

1. `public OutputStreamWriter(OutputStream os)` :   用当前默认编码UTF-8把字节输出流转换成字符输出流
2. `public OutputStreamWriter(OutputStream os , String charset)`:指定编码把字节输出流转换成字符输出流

```java
// 1.写一个字节输出流通向文件
OutputStream os = new FileOutputStream("Day10Demo/src/dlei07.txt");

// 2.把字节输出流转换成字符输出流。
// Writer fw = new OutputStreamWriter(os); // .把字节输出流按照默认编码UTF-8转换成字符输出流。
Writer fw = new OutputStreamWriter(os,"GBK"); // .  把字节输出流按照指定编码GBK转换成字符输出流。
fw.write("abc我是中国人");
fw.close();
```

