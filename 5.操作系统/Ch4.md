# Ch4 Threads

单线程和多线程类型的进程对比：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208162116659.png" alt="image-20201208162116659" style="zoom:67%;" />

##### 1.多线程的好处

①响应性responsiveness，易引起反应：适合交互式应用和层序interactive application；②共享资源：多个线程之间可以共享code和dat；③经济性：创建进程耗时，相比于线程更加expensive；④利用MP架构：多线程增加并发性和利用率

##### 2.进程和线程的比较

①进程通常是独立的，而线程作为进程的子集存在。②进程中携带的状态信息比线程多得多，而进程中的多个线程共享进程状态以及进程内存和其他资源。③进程具有单独的地址空间，而线程共享它们的地址空间；③进程仅通过系统提供的进程间通信机制进行交互，而同一进程中线程之间的上下文切换通常比进程之间的上下文切换更快。

##### 3.两种线程类型

user threads: 由用户级user-level线程库thread library进行线程管理。三个主线程库：POSIXP线程（也可以作为系统库提供）Win32线程和Java线程。

kernel threads: 内核态线程。由内核支持。几乎所有当代OS都实现了内核线程。如Windows XP/2000，Solaris，Linux，Tru64UNIX，Mac OS X

## 多线程模型multithreads models

##### 1.Many-to-One

许多用户级线程user-level threads映射到单个内核kernel线程。线程mgmt是有效的，但如果进行系统调用，内核kernel 一次只能调度一个线程。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208163454369.png" alt="image-20201208163454369" style="zoom:67%;" />

##### 2.One-to-one

每个用户态线程映射到一个内核态线程上。多并发more concurrency，但是创建线程的代价高。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208163544517.png" alt="image-20201208163544517" style="zoom:67%;" />

##### 3.Many-to-Many

多个用户态线程映射到多个kernel内核线程。允许操作系统创建足够多的内核态线程。更灵活

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208163727686.png" alt="image-20201208163727686" style="zoom:67%;" />

##### 4.Two-level Model

在Many-to-many的基础上允许一个用户态线程被限制在一个内核态线程上。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208163957865.png" alt="image-20201208163957865" style="zoom:67%;" />

## 线程操作Threading Issues

##### 1.系统调用的语义分析 Semantics of fork() and exec() system calls

fork()只复制调用fork()线程还是所有线程？ 一些unix系统有两个版本的fork，一个复制所有线程，另一个只复制调用fork()的线程。Exec()将取代整个流程。

##### 2.线程撤销Thread cancellation

线程撤销了，线程使用的资源是否要撤销？在线程完成之前终止它有两种一般方法：①异步取消Asynchronous cancellation--立即终止目标线程；②延迟取消Deferred cancellation: 允许目标线程通过设立一个flag定期检查决定是否应该终止线程。

##### 3.Signal handling

信号Signal在UNIX系统中用于通知进程某一特定事件已经发生。

信号处理程序signal handler用于处理信号。处理的三个阶段：①信号signal由特定事件产生；②信号传递给进程；③信号被处理。

接收到signal后是一个线程响应还是所有线程响应？信号传递的几种模式(传递方法取决于信号的类型)：①将信号传递给信号将被应用到的线程；②将信号传递给进程中的每个线程；③将信号传递给进程中的某些线程；④分配一个特定的线程来接收进程种产生的所有信号。

##### 4.Thread pools

如果按照进程一样的方式，每次创建或者销毁一个进程就在内核态中创建和销毁一个TCB(thread control block)，会大大影响效率，所以需要使用进程池，免去频繁创建和销毁对象。

在池中pool创建多个线程来等待去工作wait to work。优点：①已经存在的线程通常比创建新线程能更快地为请求request提供服务；②这样就可以将该应用程序中的线程数量限制为池的大小。

![image-20201208165434945](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208165434945.png)

##### 5.线程特定的数据Thread-specific data

允许每个线程都有自己的数据副本。当没有创建线程创建的控制权时（即，只能使用线程池时来产生新线程时）是很有用的。①与进程不同，单个程序中的所有线程共享相同的地址空间，这意味着，如果一个线程修改内存中的位置（如全局变量)，则所有其他线程都可以看到visiable更改。②线程特定的数据区thread-specific data area：存储在该区域中的变量对每个线程都有副本，并且每个线程可以在不影响其他线程的情况下修改该变量的副本（其他线程的该副本没有变）。

##### 6.Scheduler activations

many-to-many和两级模型two-level都需要通信来保持分配给该应用程序的内核线程在一个适当的数量。

LWP（轻量级进程light-weight process）是一个附加到内核线程的虚拟处理器：①调度器Scheduler激活以提供upcalls-一种从内核到线程库的通信机制；②线程库通过一个upcall处理程序handler处理upcalls。这种通信允许应用程序在应用程序线程即将阻塞时维护正确数量的内核线程：应用线程即将阻塞时触发upcall。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211222639415.png" alt="image-20201211222639415" style="zoom:67%;" />

##### 7.Pthreads

是用于创建和同步线程的POSIX标准API。既可以是用户level也可以是kernel level的库。API指定线程库的行为，具体实现取决于库的开发。常见于UNIX操作系统(Solaris, Linux, MaxOS)

## 不同操作系统的线程设计

##### 1.Windows XP Threads

实现了one-to-one mapping。每个线程包含：①thread id；②寄存器集合register set；③用户态和内核态各自的栈separate user and kernel stacks；④私有数据存储区private data storage area. ②③④寄存器集、堆栈和私有存储区域称为线程的上下文context。

线程主要的数据结构包括：①ETHREAD(executive thread block)②KTHREAD(kernel thread block)③TEB(thread environment block)

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211223526067.png" alt="image-20201211223526067" style="zoom:67%;" />

##### 2.Linux Threads

Linux中称线程为任务task；线程通过系统调用system call--clone()来创建。clone()允许子任务(线程)共享父任务(线程)的地址空间。

##### 3.Java Threads

Java线程由JVM管理。Java线程可以通过①扩展线程类(extending thread class)或②实现runnable接口创建

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211223842465.png" alt="image-20201211223842465" style="zoom:67%;" />

















