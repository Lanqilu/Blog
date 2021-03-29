---
title: Lambda表达式
date: 2021-03-16 19:18:43
tags:
- Java
- Lambda
- 函数式编程
categories:
- Java
---

## 为什么引入Lambda表达式

Lambda表达式时一个可传递的<font color=#FF6666>代码块</font>。Lambda 表达式以字面量的形式把少量代码直接写在程序中，而且让 Java 编程更符合函数式风格。

对于 Lambda 表达式的很多功能都能使用嵌套类型通过回调和处理程序等模式实现，但使用的句法总是非常冗长，尤其是，就算只需要在回调中编写一行代码，也要完整定义一个新类型。

## Lambda表达式的语法

Lambda表达式的格式:

```
(匿名内部类被重写方法的形参列表) -> {
    被重写方法的方法体代码。
}
```


Lambda表达式的使用前提：
1. Lambda表达式并不能简化所有匿名内部类的写法。
2. Lambda表达式只能简化接口中只有一个抽象方法的匿名内部类形式。




这里使用对数组遍历的例子来展示 Lambda 表达式是如何一步步的来减少代码量

```Java
public class LambdaDemo {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("Halo");
        names.add("Tom");
        names.add("张三");

        names.forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        });

        names.forEach((String s) -> {
                System.out.println(s);
        });

        names.forEach((s) -> {
            System.out.println(s);
        });

        names.forEach(s -> {
            System.out.println(s);
        });

        names.forEach(s -> System.out.println(s) );

        // 方法引用
        names.forEach(System.out::println);
    }
}

```

1. 如果 Lambda 表达式的方法体代码只有一行代码。可以省略大括号不写，同时要省略分号
2. 如果 Lambda 表达式的方法体代码只有一行代码。可以省略大括号不写。此时，如果这行代码是 `return` 语句，必须省略 `return` 不写，同时也必须省略 `;` 不写
3. 参数类型可以省略不写。
4. 如果只有一个参数，参数类型可以省略，同时 `()` 也可以省略。

这种句法能通过一种十分紧凑的方式表示简单的方法，而且能很大程度上避免使用匿名类。