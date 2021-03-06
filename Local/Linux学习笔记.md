---
title: Linux学习笔记
date: 2020-01-03 14:51:35
tags:
- Linux
categories:
- Linux
password:
---

{% note info no-icon %}
用于记录Linux学习过程,基于CentOS
{% endnote %}

<!-- more -->

---

## Linux简单介绍

Linux可划分为以下四个部分：

+ Linux内核：主要负责系统内存管理、软件程序管理、硬件设备管理和文件系统管理四种功能
+ GNU工具：
+ 图形化桌面环境
+ 应用软件

![Linux系统](http://img.whl123456.top/image/Linux.png)



### 内存管理系统

Linux内核不仅管理服务器上的可用物理内存，还可以创建和管理虚拟内存。

内核通过硬盘上的存储空间来实现虚拟内存，这块区域称为**交换空间**（swap space）。

内存存储单元按组划分成很多块，这些块称作页面（page）。内核将每个内存页面放在物理 内存或交换空间。然后，内核会维护一个内存页面表，指明哪些页面位于物理内存内，哪些页面被换到了磁盘上。

内核会记录哪些内存页面正在使用中，并自动把一段时间未访问的内存页面复制到交换空间区域（称为换出，swapping out）——即使还有可用内存。当程序要访问一个已被换出的内存页面时，内核必须从物理内存换出另外一个内存页面给它让出空间，然后从交换空间换入请求的内存页面。显然，这个过程要花费时间，拖慢运行中的进程。只要Linux系统在运行，为运行中的程序换出内存页面的过程就不会停歇。

### 软件程序管理

Linux操作系统将运行中的程序称为**进程**。内核控制着Linux系统如何管理运行在系统上的所有进程。

内核创建了第一个进程（称为init进程）来启动系统上所有其他进程。

> 脚本通过到/etc/init.d目录下的入口启动

当内核启动时，它会将init进程加载到虚拟内存中。内核在启动任何其他进程时，都会在虚拟内存中给新进程分配一 块专有区域来存储该进程用到的数据和代码。

### 硬件设备管理

任何Linux系统需要与之通信的设备，都需要在内核代码中加入其驱动程序代码。驱动程序代码相当于应用程序和硬件设备的中间人，允许内核与设备之间交换数据。

在Linux内核中有两种方法用于插入设备驱动代码：

+ 编译进内核的设备驱动代码
+ 可插入内核的设备驱动模块

开发人员提出了内核模块的概念。它允许将驱动代码插入到运行中的内核而无需重新编译内 核。

Linux系统将硬件设备当成特殊的文件，称为设备文件。设备文件有3种分类：

+ 字符型设备文件
+ 块设备文件
+ 网络设备文件

字符型设备文件是指处理数据时每次只能处理一个字符的设备。大多数类型的调制解调器和终端都是作为字符型设备文件创建的。

块设备文件是指处理数据时每次能处理大块数据的设备，比如硬盘。

网络设备文件是指采用数据包发送和接收数据的设备，包括各种网卡和一个特殊的回环设备。这个回环设备允许Linux系统使用常见的网络编程协议同自身通信。

Linux为系统上的每个设备都创建一种称为**节点**的特殊文件。与设备的所有通信都通过设备节点完成。每个节点都有唯一的数值对供Linux内核标识它。数值对包括一个主设备号和一个次设备号。类似的设备被划分到同样的主设备号下。次设备号用于标识主设备组下的某个特定设备。

### 文件系统管理

![Linux文件系统](http://img.whl123456.top/image/image-20200611200821228.png)

## 注意事项

+ Linux严格区分大小写

+ Linux中所有的内容以文件形式保存，包括硬件

+ Linux不靠扩展名区分文件类型，基于权限

  > 压缩包：.gz、.bz2、.tar.bz2、.tgz
  > 二进制包：.rpm
  > 脚本文件：.sh
  > 配置文件：.conf

+ 设备需要挂载后才能使用

+ Linux使用正斜线（\）在文件路径中划分目录，反斜线（/）用来标志转义字符

## Linux各目录的作用

+ /bin/：存放系统命令目录，所有用户都可以执行
+ /sbin/：只有root可以设置
+ /usr/bin/：存放与系统启动无关的命令，单用户下无法执行
+ /usr/sbin/：只有root可以设置
+ /boot/：系统启动目录，保存系统相关的文件
+ /dev/：设备保存位置
+ /etc/：配置文件保存位置
+ /home/：普通用户目录
+ /lib/：系统调用的函数库
+ /opt/：第三方安装的软件保存位置，习惯把软件放置到/usr/local/
+ /lost+found/：这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件
+ /root/：该目录为系统管理员，也称作超级权限者的用户主目录。
+ /proc/：虚拟文件系统，设备状态信息，数据保存在内存中
+ /sys/：保存内核相关信息
+ /root/：超级用户
+ /srv/：服务数据目录
+ /tmp/：临时目录，可以清空
+ /usr/：系统软件资源目录
+ /var/：动态数据保存位置，日志，软件运行产生的文件

## Linux常用命令

命令格式：命令 [-选项]  [参数]

### 目录处理

1. 显示目录文件：`ls`

   命令英文原意：list

    + `-a` 显示所有文件，包括隐藏文件
    + `-l` 显示详情信息[h]人性化显示（可以直接使用`ll`）
    + `-d` 查看目录属性
    + `-i` 文件ID号

   > -rw-r--r--
   >
   > 第1个“-”文件类型包括：-二进制文件，d目录，l软连接文件
   >
   > 第2-4个所有者
   >
   > 第5-7个所属组
   >
   > 第8-10个其他人
   >
   > r读、w写、x执行

2. 创建目录：`mkdir`
   命令英文原意：make directories

   + `-p` 递归创建

3. 切换目录：`cd`

   命令英文原意：change directory

4. 查看当前目录：`pwd`

5. 删除空目录：`rmdir`
   命令英文原意：remove empty directories

6. 复制文件或目录：`cp`
   命令英文原意：copy

   + `-rp`[原目录] [目标目录]
   + `-r` 复制目录
   + `-p` 保留文件属性

7. 剪切或改名文件：`mv`
   命令英文原意：move

   + mv [原目录] [目标目录]

8. 删除文件：`rm`
   命令英文原意：remove

   + `-r` 删除目录
   + `-f` 强制删除

### 文件处理

1. 创建空文件：`touch`
2. 显示文件内容：`cat` ；`tac`（反向展示）[-n]
3. 分页显示文件内容：more；less
   + 空格或f翻页
   + 回车换行
   + q退出
   + less b向上翻页
   + less 查找n
4. 显示文件前几行：`head` [-n]指定行数
5. 显示文件后几行：`tail` [-f] 查看动态变化[-n]

### 链接命令

生成链接文件：`ln`（link）[-s]创建软链接
