# Cheapter1 Computers, programs and Java

## What is Computer

电脑由CPU,存储器memory，硬盘hard disk，软盘floppy disk，显示器monitor,打印机printer和通讯设备communication devices组成

##### 1.CPU

central processing unit,从内存中取retrieve指令并执行，速度单位megahertz(MHz)，1 gigahertz = 1000 megahertz

##### 2.Memory

存储数据和CPU执行的程序指令，程序和数据在执行前必须送到内存中。<font color="red">A memory byte is never empty but its initial content may be meaningless to your program</font>. 主要存储设备：disk drivers(hard disks and floppy disks), CD drivers(CD-R and CD-RW), and Tape drivers

##### 3.Monitor

resolution: 显示设备水平和垂直方向上的像素点个数Pixels, picture elements

dot pitch：像素点之间的空间大小，越小越清晰

##### 4.通讯设备Communication Devices

## programs

##### 1.三类语言

机器语言machine language：二进制形式的原始指令

汇编语言assembly language：简化机器语言，汇编器assembler

高级语言：English-like and easy to learn and program，写成的source code

##### 2.解释和编译源码Interpreting/ Compiling Source code

解释器interpreter：一条一条statements的读入源码，翻译成机器码或虚拟机码virtual machine code，然后立即执行executes it right away.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215192738008.png" alt="image-20201215192738008" style="zoom:67%;" />

编译器compiler：将整个源码翻译成机器码文件machine-code file，然后再被执行。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215192912807.png" alt="image-20201215192912807" style="zoom:67%;" />

##### 3.为什么学习Java?

Java允许用户能够在互联网上开发和部署服务器servers，桌面应用desktop computers和小型手持设备small hand-held devices的应用程序。Java是互联网编程语言。是通用编程语言general purpose，互联网编程语言Internet programming language.

##### 4.Java的历史：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215193320808.png" alt="image-20201215193320808" style="zoom: 67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215193346556.png" alt="image-20201215193346556" style="zoom:67%;" />

##### 5.Java的特性

没有指针、多重继承、操作符重载

运行Java程序需要一个解释器。程序被编译成Java虚拟机代码: 字节码bytecode。 字节码独立于机器，可以在任何有Java解释器的机器上运行，这是Java虚拟机(JVM)的一部分)。

强类型机制、异常处理、垃圾内存自动搜集等

解释性语言，无指针，数据越界检查等

多线程编程

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215193458257.png" alt="image-20201215193458257" style="zoom:67%;" />

##### 6.Java程序的执行过程：

源码.java文件先被JVM上的编译器编译成字节码bytecode，即.class文件，被存储在磁盘上，然后bytecode再被解释器interpreter解释执行。字节码可以在任何有JVM虚拟机的计算机上运行。Java Virtual Machine是一个解释interprets Java字节码的软件。

![image-20201215193901923](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215193901923.png)

![image-20201215194415580](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215194415580.png)

##### 7.Java程序的解剖分析anatomy

类名Class name：每个Java program至少一个类class, 类名通常以大写字母开头

Main method：类中必须包含主方法main, 主方法是程序的执行入口。

Reserved words：保留的单词或关键字是对编译器具有特定意义的单词，不能用于程序中的其他目的。

注释规范：程序开始包含一个摘要，解释程序所作的事情，关键特性，支持的数据结构和独特之处，包括姓名，类，日期等概述；

命名规范：①选择有意义、描述性的类名；②类名首字母最好大写

留白规范：使用空行分隔代码段

块规范block styles：括号最好使用end-of-line形式

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215195427034.png" alt="image-20201215195427034" style="zoom:80%;" />

##### 8.编程错误programming errors

语法错误syntax error--可被编译器检测到

运行时错误runtime error--导致程序终止abort，如

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215195622893.png" alt="image-20201215195622893" style="zoom: 50%;" />

逻辑错误logic error--运行的结果和预定的不同









