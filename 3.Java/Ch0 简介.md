# Ch0: 简介

## Java的三个体系

--JavaSE桌面应用

--JavaEE是JavaSE的扩展，企业级应用

--JavaME移动设备，手机类嵌入式编程是对JavaSE的缩减

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215185959642.png" alt="image-20201215185959642" style="zoom:67%;" />

## Compiled VS Interpreted

##### 1.Compile

Compilers将源代码source program翻译成机器语言machine language，通常将源码翻译成object code，object code们再通过链接器linker链接在一起。速度上，编译器更胜一筹。过程如下图：

![image-20201215190633677](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215190633677.png)

源码被放到不同的平台，然后根据平台特性编译成适合该平台的机器语言(二进制语言)，二进制文件只能在该特定的平台上执行。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215190948961.png" alt="image-20201215190948961"  />

##### 2.Java--解释型语言

安全性能上，fully interpreted languages wins。

Java源码(.java文件)先被编译器Java compiler编译成字节码bytecode(.class文件)，字节码被Java的虚拟机virtual machine，即JVM解释，可以在任何平台运行。Java实际上是在速度和安全性能之间的权衡.

实现了：wirte once, compile, run anywhere

![image-20201215191323321](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215191323321.png)

![image-20201215191409589](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215191409589.png)

##### 3.字节码bytecode的优势：

字节码独立于体系结构architecture independent，为每个体系结构编写虚拟机比编译器更容易。

虚拟机会将执行频率高的方法或语句块通过(Just-in-time compiler)JIT编译成本地机器码，提高了代码效率。执行时JIT会把翻译过的机器码保存起来以备下次使用。









