---
title: Lombok入门
date: 2020-11-03 23:28:43
tags:
- Java
- Lombok
categories:
- Java
---

<!--more-->

----

## 概述

[Lombok](https://github.com/rzwitserloot/lombok) 是一个 Java 工具，通过使用其定义的注解，自动生成常见的冗余代码，提升开发效率。

举个例子，在 Java [POJO](https://baike.baidu.com/item/POJO) 类上，添加 `@Setter` 和 `@Getter` 注解，自动生成 set、get 方法的代码。示例如下：

```java
// 我们编写的 UserDO.java 代码
@Setter
@Getter
public class UserDO {

    private String username;
    private String password;

}

// 实际生成的代码（通过 UserDO.class 反编译）
public class UserDO {
    private String username;
    private String password;

    public UserDO() {
    }

    public void setUsername(final String username) {
        this.username = username;
    }

    public void setPassword(final String password) {
        this.password = password;
    }

    public String getUsername() {
        return this.username;
    }

    public String getPassword() {
        return this.password;
    }
}
```

## 实现原理

Lombok 的实现原理，基于 [JSR269(Pluggable Annotation Processing API)](https://jcp.org/en/jsr/detail?id=269) 规范，自定义编译器注解处理器，用于在 Javac 编译阶段时，扫描使用到 Lombok 定义的注解的类，进行自定义的代码生成。

## 安装使用 Lombok

在 IDEA 中，已经提供了 [IntelliJ Lombok plugin](https://plugins.jetbrains.com/plugin/6317-lombok) 插件，方便我们使用 Lombok。安装方式很简单，只需要在 IDEA Plugins 功能中，搜索 Lombok 关键字即可。

安装完成，需要重启 IDEA 来让该插件生效。生效完成后，我们可以在 IDEA 的设置中，找到 IDEA Lombok 功能。

在 Maven 项目的 `pom` 文件，我们只需要引入 [`lombok`](https://mvnrepository.com/artifact/org.projectlombok/lombok) 依赖，就可以使用 Lombok 啦。代码如下：

```xml
<!-- 引入 Lombok 依赖 -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
```

## Lombok 注解一览

[`@Getter`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/Getter.java) 注解，添加在**类**或**属性**上，生成对应的 get 方法。

[`@Setter`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/Setting.java) 注解，添加在**类**或**属性**上，生成对应的 set 方法。

[`@ToString`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/ToString.java) 注解，添加在**类**上，生成 toString 方法。

[`@EqualsAndHashCode`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/EqualsAndHashCode.java) 注解，添加在**类**上，生成 equals 和 hashCode 方法。

`@AllArgsConstructor`、`@RequiredArgsConstructor`、`@NoArgsConstructor` 注解，添加在**类**上，为类自动生成对应参数的构造方法。

[`@Data`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/Data.java) 注解，添加在**类**上，是 5 个 Lombok 注解的组合。

- 为所有属性，添加 `@Getter`、`@ToString`、`@EqualsAndHashCode` 注解的效果
- 为非 `final` 修饰的属性，添加 `@Setter` 注解的效果
- 为 `final` 修改的属性，添加 `@RequiredArgsConstructor` 注解的效果

`@Value` 注解，添加在**类**上，和 `@Data` 注解类似，区别在于它会把所有属性默认定义为 `private final` 修饰，所以不会生成 set 方法。

[`@CommonsLog`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/extern/apachecommons/CommonsLog.java)、[`@Flogger`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/extern/flogger/Flogger.java)、[`@Log`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/extern/java/Log.java)、[`@JBossLog`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/extern/jbosslog/JBossLog.java)、[`@Log4j`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/extern/log4j/Log4j.java)、[`@Log4j2`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/extern/log4j/Log4j2.java)、[`@Slf4j`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/Slf4j.java)、[`@Slf4jX`](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/Slf4jX.java) 注解，添加在**类**上，自动为类添加对应的日志支持。

`@NonNull` 注解，添加在**方法参数**、**类属性**上，用于自动生成 `null` 参数检查。若确实是 `null` 时，抛出 NullPointerException 异常。

`@Cleanup` 注解，添加在方法中的**局部变量**上，在作用域结束时会自动调用 `#close()` 方法，来释放资源。例如说，使用在 Java IO 流操作的时候。

`@Builder` 注解，添加在**类**上，给该类加个构造者模式 Builder 内部类。

`@Synchronized` 注解，添加在**方法**上，添加同步锁。

`@SneakyThrows` 注解，添加在**方法**上，给该方法添加 `try catch` 代码块。

`@Accessors` 注解，添加在**方法**或**属性**上，并设置 `chain = true`，实现链式编程。