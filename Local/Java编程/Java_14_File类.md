---
title: File类
date: 2021-03-17 14:44:43
tags:
- Java
categories:
- Java
---

## File类的概述

File类：代表操作系统的文件对象。是用来操作操作系统的文件对象的，删除文件，获取文件信息，创建文件（文件夹）...

广义来说操作系统认为文件包含（文件和文件夹）

File类的创建文件对象的API：

包：`java.io.File`

构造器：

1. `public File(String pathname)`:根据路径获取文件对象
2. `public File(String parent , String child)`:根据父路径和文件名称获取文件对象！
3. `public File(File parent , String child)`

File类创建文件对象的格式:

1. `File f = new File("绝对路径/相对路径");`
2. `File f = new File("文件对象/文件夹对象");`

> 绝对路径：从磁盘的的盘符一路走到目的位置的路径。
>
> 1. 绝对路径依赖具体的环境，一旦脱离环境，代码可能出错！！
> 2. 一般是定位某个操作系统中的某个文件对象。
>
> 相对路径：不带盘符的。（重点）
>
> 1. 默认是直接相对到工程目录下寻找文件的。
> 2. 相对路径只能用于寻找工程下的文件。
> 3. 能用相对路径就应该尽量使用，可以跨平台！

```java
// 1.创建文件对象：使用绝对路径
File f1 = new File("D:\\itcast\\图片资源\\beautiful.jpg");
File f0 = new File("D:"+File.separator+"itcast"+File.separator+"图片资源"+File.separator+"beautiful.jpg");
// 获取文件的大小，字节大小
System.out.println(f1.length()); 
```

> 文件路径分隔符：
>
> 1. .使用正斜杠： `/`
> 2. .使用反斜杠： `\`(需要转义)
> 3. 使用分隔符API：`File.separator`

```java
// 2.创建文件对象：使用相对路径
File f2 = new File("Day09Demo/src/dlei01.txt");
System.out.println(f2.length());
```

```java
// 3.创建文件对象：代表文件夹。
File f3 = new File("D:\\itcast\\图片资源");
// 判断路径是否存在！！
System.out.println(f3.exists());
```

## File常用API

### File类的获取功能的API

1. `public String getAbsolutePath()`  ：返回此File的绝对路径名字符串。
2.  `public String getPath()`  ： 获取创建文件对象的时候用的路径
3. `public String getName()`  ： 返回由此File表示的文件或目录的名称。
4. `public long length()`  ：    返回由此File表示的文件的长度。

```java
// 1.绝对路径创建一个文件对象
File f1 = new File("D:/itcast/图片资源/meinv.jpg");
// a.获取它的绝对路径。
System.out.println(f1.getAbsolutePath());
// b.获取文件定义的时候使用的路径。
System.out.println(f1.getPath());
// c.获取文件的名称：带后缀。
System.out.println(f1.getName());
// d.获取文件的大小：字节个数。
System.out.println(f1.length());
```

```java
// 2.相对路径
File f2 = new File("Day09Demo/src/dlei01.txt");
// a.获取它的绝对路径。
System.out.println(f2.getAbsolutePath());
// b.获取文件定义的时候使用的路径。
System.out.println(f2.getPath());
// c.获取文件的名称：带后缀。
System.out.println(f2.getName());
// d.获取文件的大小：字节个数。
System.out.println(f2.length());
```

### File类的判断功能的方法

1. `public boolean exists()` ：此File表示的文件或目录是否实际存在。
2. `public boolean isDirectory()`：此File表示的是否为目录。
3. `public boolean isFile()` ：此File表示的是否为文件
4.  `public boolean isDirectory()`：此File是否是文件夹

```java
// 1.文件对象。
File f1 = new File("D:\\itcast\\图片资源\\meinv.jpg");
// a.判断文件路径是否存在
System.out.println(f1.exists()); // true
// b.判断文件对象是否是文件,是文件返回true ,反之
System.out.println(f1.isFile()); // true
// c.判断文件对象是否是文件夹,是文件夹返回true ,反之
System.out.println(f1.isDirectory()); // false
```

```java
// 2.文件夹对象。
File f2 = new File("D:\\itcast\\图片资源");
// a.判断文件路径是否存在
System.out.println(f2.exists()); // true
// b.判断文件对象是否是文件,是文件返回true ,反之
System.out.println(f2.isFile()); // false
// c.判断文件对象是否是文件夹,是文件夹返回true ,反之
System.out.println(f2.isDirectory()); // true
```

### File类的创建和删除的方法

1.  `public boolean createNewFile()` ：当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。 （几乎不用的，因为以后文件都是自动创建的！）
2. `public boolean delete()` ：删除由此File表示的文件或目录。 （只能删除空目录）
3. `public boolean mkdir()` ：创建由此File表示的目录。（只能创建一级目录）
4. `public boolean mkdirs()` ：可以创建多级目录（建议使用的）

```java
File f = new File("Day09Demo/src/dlei02.txt");
// a.创建新文件，创建成功返回true ,反之
System.out.println(f.createNewFile());

// b.删除文件或者空文件夹
System.out.println(f.delete());
// 不能删除非空文件夹，只能删除空文件夹
File f1 = new File("D:/itcast/aaaaa");
System.out.println(f1.delete());

// c.创建一级目录
File f2 = new File("D:/itcast/bbbb");
System.out.println(f2.mkdir());

// d.创建多级目录
File f3 = new File("D:/itcast/e/a/d/ds/fas/fas/fas/fas/fas/fas");
System.out.println(f3.mkdirs());
```

### File针对目录的遍历

1. `public String[] list()`：获取当前目录下所有的“一级文件名称”到一个字符串数组中去返回。
2. `public File[] listFiles()`(常用)：获取当前目录下所有的“一级文件对象”到一个文件对象数组中去返回（重点）

```java
File dir = new File("D:\\itcast");
// a.获取当前目录对象下的全部一级文件名称到一个字符串数组返回。
String[] names = dir.list();
for (String name : names) {
    System.out.println(name);
}
// b.获取当前目录对象下的全部一级文件对象到一个File类型的数组返回。
File[] files = dir.listFiles();
for (File file : files) {
    System.out.println(file.getAbsolutePath());
}

// ---------拓展------------
// 获取最后修改时间
File f1 = new File("D:\\itcast\\图片资源\\beautiful.jpg");
long time = f1.lastModified(); // 最后修改时间！
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
System.out.println(sdf.format(time));
```

## 递归实现文件搜索(非规律递归)

```java
public static void main(String[] args) {
    // 搜索调用方法
    searchFiles(new File("D:/soft") , "eclipse.exe");
}

/**
 * 去某个目录下搜索某个文件
 * @param dir 搜索文件的目录。
 * @param fileName 搜索文件的名称。
 */
public static void searchFiles(File dir , String fileName){
    // 1.判断是否存在该路径，是否是文件夹
    if(dir.exists() && dir.isDirectory()){
        // 2.提取当前目录下的全部一级文件对象
        File[] files = dir.listFiles(); // null/[]
        // 3.判断是否存在一级文件对象（判断是否不为空目录）
        if(files!=null && files.length > 0){
            // 4.判断一级文件对象
            for (File f : files) {
                // 5.判断file是文件还是文件夹
                if(f.isFile()){
                    // 6.判断该文件是否为我要找的文件对象
                    if(f.getName().contains(fileName)){
                        System.out.println(f.getAbsolutePath());
                        try {
                            // 启动它（拓展）
                            Runtime r = Runtime.getRuntime();
                            r.exec(f.getAbsolutePath());
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }else{
                    // 7.该文件是文件夹，文件夹要递归进入继续寻找
                    searchFiles(f ,fileName);
                }
            }
        }
    }
}
```

