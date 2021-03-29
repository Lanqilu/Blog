---
title: Java并发编程
tags:
  - Java
categories:
  - [Java, 并发线程]
date: 2020-04-06 22:05:45
---

多任务是操作系统的一种能力，看起来可以在同一时刻运行多个程序。但并发执行的<font color="#FF6666">进程</font>数目并不受限与CPU数目。操作系统会为每个进程分配CPU时间片，给人并行处理的感觉。

多线程程序在更低一层扩展了多任务的概念程序看起来在同时完成多个任务。每个任务在一个<font color="#FF6666">线程</font>（thread）中执行，线程是控制线程的简称。如果一个任务可以同时运行多个线程，则称这个程序是多线程的。

多线程与多进程的区别：

1. 本质区别在于每个进程都拥有自己的一整套变量，而线程则共享数据。
2. 共享变量使线程之间的通信比进程之间的通信更有效、更容易。
3. 此外，在有些操作系统中，与进行相比较，线程更“轻量级”，创建、撤销一个线程比启动新进程的开销要小得多。

---

## 简介

### 并发简史

之所以在计算在中加入操作系统来实现多个程序的问时执行，主要是基于以下原因

1. **资源利用率**。在某些情况下，程序必续等待某个外部操作执行完成，例如输入操作或输出操作等，而在等待时程序无法执行其他任何工作。因此，如果在等待的同时可以运行另一个程序，那么无疑将提高资源的利用率。
2. **公平性**。不同的用户和程序对于计算机上的资源有着同等的使用权。一种高效的运行方式是通过粗粒度的时间分片（ Time Slicing ）使这些用户和程序能共享计算机资源，而不是由一个程序从头运行到尾，然后再启动下一个程序。
3. **便利性**。通常来说，在计算多个任务时，应该编写多个程序，每个程序执行一个任务并在必要时相互通信，这比只编写一个程序来计算所有任务更容易实现。





什么是进程？ 程序是静止的，运行中的程序就是进程。

进程的三个特征：

1.动态性 ： 进程是运行中的程序，要动态的占用内存，CPU和网络等资源。

2.独立性 ： 进程与进程之间是相互独立的，彼此有自己的独立内存区域。

3.并发性 ： 假如CPU是单核，同一个时刻其实内存中只有一个进程在被执行。CPU会分时轮询切换依次为每个进程服务，因为切换的速度非常快，给我们的感觉这些进程在同时执行，这就是并发性。

并行:同一个时刻同时有多个在执行。




```java
// 多线程创建
/*  1. 创建一个继承与Thread类的子类
    2. 重写Thread类的run()方法
    3. 创建Thread类的子类的对象
    4. 通过此对象调用start()*/

public class CreateThread {

    public static void main(String[] args) {
        //3.创建Thread类的子类的对象
        MyThread t1 = new MyThread();
        //通过此对象调用start()
        t1.start();//线程开始执行或调用当前线程的run()方法
//        t1.run();//直接调用run()方法是单线程

        //获取当前线程的名字
        System.out.println("Thread.currentThread().getName() = " + Thread.currentThread().getName());

        System.out.println("hello");//hello先输出

        //再创建线程
//        t1.start();//报错:IllegalThreadStateException
        //重新创建一个线程对象
        MyThread t2 = new MyThread();
        t2.start();
    }
}

//1.创建一个继承与Thread类的子类
class MyThread extends Thread {
    //2.重写Thread类的run()方法
    //将此线程的操作声明在run()中

    @Override// run+Enter快捷方式
    public void run() {
        //获取当前线程的名字
        System.out.println("Thread.currentThread().getName() = " + Thread.currentThread().getName());
        for (int i = 0; i < 50; i++) {
            if (i % 2 == 0) {
                System.out.println("i = " + i);
            }
        }
    }
}
```

```java
// 创建两个线程一个遍历偶数一个遍历奇数
public class CreateThreadTest {
    public static void main(String[] args) {
        evenThread t1 = new evenThread();
        oddThread t2 = new oddThread();
        t1.start();
        t2.start();

        //简便方式:匿名子类
        new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    if (i % 2 == 0) {
                        System.out.println(Thread.currentThread().getName() + "：i = " + i);
                    }
                }
            }
        }.start();
    }
}

//遍历偶数
class evenThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + "：i = " + i);
            }
        }
    }
}

//遍历奇数
class oddThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 != 0) {
                System.out.println(Thread.currentThread().getName() + "：i = " + i);
            }
        }
    }
}
```

```java
// Thread中常用方法
/*
* getPriority() 获取线程优先级
* setPriority() 改变线程优先级
*   MAX_PRIORITY --> 10
*   MIN_PRIORITY --> 1
*   NORM_PRIORITY --> 5
* setName()     设置线程名
* sleep()       睡眠指定时间
* join()        阻塞
* isAlive()     查看线程状态
* yield()       释放当前CPU的使用权
* */

public class ThreadMethod {
    public static void main(String[] args) {
        thread t1 = new thread("0");
        t1.setName("A");//设置线程名
        t1.start();

        System.out.println("t1.isAlive() = " + t1.isAlive());

        //主线程命名
        Thread.currentThread().setName("主线程");
        //获取线程名称
        System.out.println("Thread.currentThread().getName() = " + Thread.currentThread().getName());
        //获取线程优先级
        System.out.println("Thread.currentThread().getPriority() = " + Thread.currentThread().getPriority());
        //改变线程优先级
        Thread.currentThread().setPriority(Thread.MAX_PRIORITY);
        System.out.println("Thread.currentThread().getPriority() = " + Thread.currentThread().getPriority());

        for (int i = 0; i < 100; i++) {
//            try {
//                //睡眠指定时间
//                Thread.currentThread().sleep(1000);
//            }
//            catch (InterruptedException e) {
//                e.printStackTrace();
//            }
            if (i % 2 == 0) {

                System.out.println(Thread.currentThread().getName() + "：i = " + i);
            }
            if (i == 20) {
                try {
                    //阻塞直到其他线程完成
                    t1.join();
                }
                catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
        System.out.println("t1.isAlive() = " + t1.isAlive());
    }
}

class thread extends Thread {

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 != 0) {
                System.out.println(Thread.currentThread().getName() + "：i = " + i);
            }
            if (i % 10 == 0) {
                //释放当前CPU的使用权
                Thread.currentThread().yield();
            }
        }
    }

    //通过构造器对线程命名
    public thread(String name) {
        super(name);
    }
}
```

