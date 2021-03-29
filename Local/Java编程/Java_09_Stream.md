---
title: Stream流
date: 2021-03-17 13:17:43
tags:
- Java
categories:
- Java
---

## Stream流概述

什么是Stream流？

在Java 8中，得益于Lambda所带来的函数式编程，引入了一个全新的Stream流概念 ，用于解决已有<font color="#FF6666">集合或数组类库</font>有的弊端。

Stream流能解决什么问题?

可以解决已有集合类库或者数组API的弊端。Stream认为集合和数组操作的API很不好用，所以采用了Stream流简化集合和数组的操作！！

```java
// 需求：从集合中筛选出所有姓张的人出来。然后再找出姓名长度是3的人。
List<String> list = new ArrayList<>();
list.add("张无忌");
list.add("周芷若");
list.add("赵敏");
list.add("张强");
list.add("张三丰");
```

不使用Stream流的做法

```java
// a.先找出姓张的人。
List<String> zhangLists = new ArrayList<>();
for (String s : list) {
    if(s.startsWith("张")){
        zhangLists.add(s);
    }
}
System.out.println(zhangLists);

// b.张姓姓名长度为3的人
List<String> zhangThreeLists = new ArrayList<>();
for (String s : zhangLists){
    if(s.length() == 3){
        zhangThreeLists.add(s);
    }
}
System.out.println(zhangThreeLists);
```

使用Stream流的做法

```java
list.stream().filter(s -> s.startsWith("张")).filter(s -> s.length() == 3).forEach(System.out::println);
```

## Stream流的获取

Stream流式思想的核心：

1. 是先得到集合或者数组的Stream流（就是一根传送带）
2. 然后就用这个Stream流操作集合或者数组的元素。
3. 然后用Stream流简化替代集合操作的API.

```java
/** --------------------Collection集合获取流-------------------------------   */
// Collection集合如何获取Stream流。
Collection<String> c = new ArrayList<>();
Stream<String> ss = c.stream();

/** --------------------Map集合获取流-------------------------------   */
Map<String, Integer> map = new HashMap<>();
// 先获取键的Stream流。
Stream<String> keyss = map.keySet().stream();
// 在获取值的Stream流
Stream<Integer> valuess = map.values().stream();
// 获取键值对的Stream流（key=value： Map.Entry<String,Integer>）
Stream<Map.Entry<String,Integer>> keyAndValues = map.entrySet().stream();

/** ---------------------数组获取流------------------------------   */
// 数组也有Stream流。
String[] arrs = new String[]{"Java", "JavaEE" ,"Spring Boot"};
Stream<String> arrsSS1 = Arrays.stream(arrs);
Stream<String> arrsSS2 = Stream.of(arrs);
```

## Stream流的常用API

 `forEach` : 逐一处理(遍历)
 `count`：统计个数
    -- long count();
 `filter` : 过滤元素
    -- Stream<T> filter(Predicate<? super T> predicate)
 `limit` : 取前几个元素
 `skip` : 跳过前几个
 `map` : 加工方法
 `concat` : 合并流。

```java
List<String> list = new ArrayList<>();
list.add("张无忌");
list.add("周芷若");
list.add("赵敏");
list.add("张强");
list.add("张三丰");
list.add("张三丰");
```

```java
// 完整写法
list.stream().filter(new Predicate<String>() {
    @Override
    public boolean test(String s) {
        return s.length() == 3;
    }
}).filter(new Predicate<String>() {
    @Override
    public boolean test(String s) {
        return s.startsWith("张");
    }
}).forEach(new Consumer<String>() {
    @Override
    public void accept(String s) {
        System.out.println(s);
    }
});
// Lambda表达式
list.stream().filter(s -> s.length() == 3).filter(s -> s.startsWith("张")).forEach(System.out::println);
```

```java
// 统计数量
long count = list.stream().filter(s -> s.length() == 3).filter(s -> s.startsWith("张")).count();
System.out.println(count);
// 取前2个
list.stream().filter(s -> s.length() == 3).limit(2).forEach(System.out::println);
// 跳过前2个
list.stream().filter(s -> s.length() == 3).skip(2).forEach(System.out::println);
```

```java
// 需求：把名称都加上“黑马的:+xxx”
list.stream().map(a -> "黑马的："+a).forEach(System.out::println);
// 需求：把名称都加成到学生对象放上去!!
// list.stream().map(name -> new Student(name)).forEach(System.out::println);
list.stream().map(Student::new).forEach(System.out::println);
```

```java
// 数组流
Stream<Integer> s1 = Stream.of(10, 20 ,30 ,40);
// 集合流
Stream<String> s2 = list.stream();
// 合并流
Stream<Object> s3 = Stream.concat(s1,s2);
s3.forEach(System.out::println);
```

## 终结与非终结方法


终结方法：一旦Stream调用了终结方法，流的操作就全部终结了，不能继续使用，只能创建新的Stream操作。终结方法： `foreach` , `count`。

非终结方法：每次调用完成以后返回一个新的流对象，可以继续使用，支持链式编程！

```java
// foreach终结方法
list.stream().filter(s -> s.startsWith("张")).filter(s -> s.length() == 3).forEach(System.out::println);
// count终结方法
long count = list.stream().filter(s -> s.startsWith("张")).filter(s -> s.length() == 3).count();
System.out.println(count);
```

## 收集Stream流

把Stream流的数据转回成集合。

Stream的作用是：把集合转换成一根传送带，借用Stream流的强大功能进行的操作。但是实际开发中数据最终的形式还是应该是集合，最终Stream流操作完毕以后还是要转换成集合。这就是收集Stream流。

Stream流：手段。集合：才是目的。

```java
Stream<String> zhangLists = list.stream().filter(s -> s.startsWith("张"));
// 把stream流转换成Set集合。
Set<String> sets = zhangLists.collect(Collectors.toSet());
System.out.println(sets);

// 把stream流转换成List集合。
Stream<String> zhangLists1 = list.stream().filter(s -> s.startsWith("张"));
List<String> lists= zhangLists1.collect(Collectors.toList());
System.out.println(lists);

// 把stream流转换成数组。
Stream<String> zhangLists2 = list.stream().filter(s -> s.startsWith("张"));
Object[] arrs = zhangLists2.toArray();
// 可以借用构造器引用申明转换成的数组类型！！！
String[] arrs1 = zhangLists2.toArray(String[]::new);
```

