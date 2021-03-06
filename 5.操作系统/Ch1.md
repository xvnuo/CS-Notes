# Ch1: Introduction

## 操作系统引入

操作系统：是计算机使用者和计算机硬件之间充当中介intermediary的程序。

目标：执行用户程序user program，使更容易解决用户问题；使计算机易于使用；使得计算机硬件被快捷操作。

##### Fimeware VS OS

Fimeware固件: 低级代码，存储在bios/cd中，eg: 电饭煲中的控制程序

OS：系统级的软件system-level software

##### 计算机系统的结构（4个部分）

硬件：提供基本的计算机资源resources，包含CPU, memory, I/O设备

OS：控制和协调各种应用程序和用户之间硬件的使用

System and Application programs: 定义系统资源解决用户计算问题的方式，eg: word processors, compiles, Web browers, database systems, video games

Users: people, machines, other computers等与计算机产生交互作用

![计算机的4个部分](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201206212207560.png)

#### 操作系统的定义

1.resources allocator: 管理所有资源，在有效和公平使用资源的相互冲突的request之间做出决定。

2.control program: 控制程序执行，防止错误和不当使用计算机improper use

<font color="red">The one program running at all times on the computer us the kernel. Everything else is either a system program(ships with OS) or an application program.</font> 一直在计算机上运行的是kernel(内核)，其他的要么是随操作系统运行的程序要么是普通的应用程序。

##### Computer StartUp计算机的启动过程

bootstrap program在计算机启动power-up或重启reboot时被加载loaded。bootstrap program 通常被存储在ROM或EPROM中，程作固件firmware，初始化系统，加载操作系统内核kernel并开始执行。

## Computer System Organization

##### 1.Computer-system operation

一个/多个CPUs、设备控制器device controllers通过提供共享内存访问的总线bus连接。竞争内存周期competing for memory cycles的CPU和设备并发执行。

I/O 设备和CPU可以并发执行；每个设备控制器控制特定的设备类型；每个设别控制器有一个本地缓存local buffer；CPU将数据从主存储器移动到本地缓冲区；设备控制器通过造成一个中断cause an interrupt来通知CPU它已经完成所有操作。

##### 2.中断interrupt的基本概念

中断向量interrupt vector包含所有服务例程service routines的地址，中断通常通过中断向量将控制转移到服务例程中。transfer control。中断架构interrupt architecture 必须保存被中断的指令的地址。当正在处理另一个中断以防止丢失中断时，禁用传入的中断incoming interrupts are disabled. 陷阱trap是由错误error或用户请求user request(即系统调用system call)引起的软件生成中断software-generated interrupt. 操作系统是由中断驱动的interrupt drivern.

##### 3.中断的处理handling

操作系统通过存储寄存器和程序计数器program counter来保持CPU的状态。可以通过①polling by a generic routinue和②vectored interrupt system来确定发生了哪种中断。单独的代码段separate segments of code 决定了对于每种类型的中断应该采取什么行动。

Interrupt Timeline:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201206215533350.png" alt="image-20201206215533350" style="zoom:80%;" />

##### 4.两种I/O处理方式

I/O开始后，控制control在I/O完成后才返回给用户程序user program：等待指令wait instruction使CPU闲置idles the CPU直到下一个中断。等待循环wait loop(争用内存访问contention for memory access).一次最多处理一个I/O请求request, 不能同时simultaneous处理process多个I/O。

I/O开始后，控制control不需要等到I/O结束就返回给用户程序：系统调用system call请求request操作系统允许用户等待I/O完成；设备状态表device-status table 包含指示类型、地址和状态state的每个I/O设备的条目entry. 操作系统索引到index into设备状态表中,以确定设备状态并修改表项以发生中断。

两种I/O处理方式的概览：同步synchronous/异步

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201206221040148.png" alt="image-20201206221040148" style="zoom: 67%;" />

设备状态图：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201206221158586.png" alt="image-20201206221158586" style="zoom:67%;" />

##### 5.直接访问存储器DMA, Direct Memory Access Structure

用于高速I/O设备high-speed，能够以接近内存速度memory speed传输信息。设备控制器将数据块blocks从缓冲区存储buffer storage直接传输到主存储器main memory，无需CPU干预intervention。每个块block只产生一个中断interrupt而不是每个字节产生一个中断。

##### 6.存储结构/层次 storage structure/hierarchy

主存main memory: CPU可以直接访问access directly的唯一大型存储介质media。二级存储secondary storage：扩展主存储器extension of main memory, 提供大large的非易失性nonvolatile存储容量。磁盘magnetic disks：硬质金属或玻璃板覆盖磁性记录材料；表面在逻辑上划分成轨道tracks、轨道被划分成扇区sectors, 磁盘控制器disk controller决定设备和计算机之间的逻辑交互logical interaction。

存储系统按照speed\cost\volatility被划分成多个层次。缓存caching：将信息复制到更快的存储系统中；主存main memory可以看作是二级存储的缓存。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201206222808709.png" alt="image-20201206222808709" style="zoom:67%;" />

##### 7.Caching

在计算机的多个层次上执行：硬件、操作系统、软件。速度不匹配speed mismatch：将暂时在使用的信息从较慢的存储复制到更快的存储中。首先检索更快的存储(cache)确定是否有需要的数据，若有，直接从cache取走使用，若无，向低级存储中找到该数据后将该数据放到cache中，便于以后使用。缓存小于被缓存的存储：cache management和cache size and replacement policy替换策略重要。

建模：整数A从硬盘disk迁移到寄存器的过程：

多任务multitasking环境必须使用use most recent value最近使用的值无论它存储在存储层次结构中的哪里。多处理器multiprocessor环境必须在硬件中提供缓存一致性cache coherency，使得所有CPU在其缓存中具有最新的值。分布式distributed环境更加复杂：一个基准datum同时存在几个副本several copies。

## Operating System Structure操作系统结构

##### 1.multiprogramming提升效率(CPU利用率utilization)

单个user不能保持CPU和I/O设备随时都在busy工作，效率低下。multiprogramming organize jobs(code and data)so CPU always has one to execute.系统中全部工作(code and data)的子集被存放到内存中。一个job通过job scheduling被选中并开始运行。

Timesharing(multitasking)是逻辑上的扩展，CPU频繁切换作业，用户可以在其运行时和每个作业交互，创建交互式计算interactive computing(interactivity).

相应时间response time应该小于1秒；每个用户在内存中至少有一个程序在执行process；多个作业同时ready ro run时需要CPU进行调度。如果进程不能放进内存fits into memory，swapping交换将他们换入换出move in and out to run.  虚拟内存允许进行不完全在内存中执行。

multi programmed system的内存布局memory layout:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208081333106.png" alt="image-20201208081333106" style="zoom:67%;" />

##### 2.操作系统操作

中断interrupt由硬件驱动；软件错误software error或请求request产生exception或trap；别的进程问题包括无线循环infinite loop，进程相互修改或修改操作系统。

Dual-mode双模式(user mode和kernel mode)操作允许操作系统保护自己和别的系统组件: 硬件提供模式位mode bit，提供区分系统何时运行用户代码user code或内核代码kernel code的能力。一些指令被设计成特定指令privileged，只在内核模式下kernel mode执行。系统调用system call将user mode模式改为内核mode，return from call resets it to user系统调用运行结束后返回到用户模式.

user mode和kernel mode的切换：定时器timer被用来防止无线循环或进程弯曲hogging资源：特定周期后设置中断set interrupt after specific period. 操作系统递减计数器decrement counter. 当计数器减为0时产生中断；在进程调度之前设置以便在进程超出分配时间时重新获得控制权regain control或终止terminate进程。

![image-20201208083046267](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208083046267.png)

##### 3.进程管理process management

进程a process是正在执行的程序program，是一个系统中的工作单元unit of work，程序是被动的实体passive entity，进程是一个主动的实体active entity。

进程需要资源resources(包括CPU,memory, I/O, files和初始数据initialization data)来完成任务task。单线程进程single-threaded process有一个程序计数器program counter来确认下一条要执行的指令的位置。进程顺序执行指令，每次执行一条直到结束。多线程进程multi-threaded process每条线程thread都有一个PC. 通常系统有许多进程，有的用户或操作系统在一个或多个CPU下并行运行。通过在进程或线程中复用CPU实现并行。concurrency by multiplexing the CPUs among the processes / threads.

操作系统负责与进程管理process management有关的以下活动activities：①创建create和删除delete用户user和系统system进程；②暂停suspending或重新开始resuming进程；③为进程提供同步机制synchronization；④为进程提供交流机制communication；⑤为进程提供死锁处理机制deadlock handling。

##### 4.内存管理memory management

进程开始和结束后所有数据data都必须在内存中；所有指令instructions都必须在内存中才能执行；内存管理memory management决定内存中存放什么来优化CPU利用率utilization和计算机对用户的相应response。

内存管理活动：①跟踪内存的哪些部分正在被谁使用；②决定哪些进程或哪些部分和数据进入和离开内存；③根据需要分配allocate和收回deallocate内存空间。

##### 5.存储管理storage management

操作系统提供统一的uniform、逻辑logical的信息存储视图information storage：①file: 将物理属性抽象为逻辑存储单元logical storage unit；②每种介质medium由设备(如硬盘驱动器，磁带驱动器)控制，存在多种属性如访问速度access speed，容量capacity，数据传输速率data-transfer rate，访问方式access method(顺序sequential和随机random访问)

文件系统管理file-system management：①文件通常被组织到目录directories里；②在多数系统中，访问控制access control决定谁可以访问哪些内容；③操作系统活动包括：创建和删除文件和目录；操作manipulate文件或目录的原语primitives，映射map文件到二级存储secondary storage，备份文件backup到稳定非易失性的存储介质上stable, non-volatile storage media.

##### 6.Mass-storage management

磁盘disk通常被用来存储放不进not fit into内存的数据或需要保存较长时间的数据。适当管理至关重要；计算机操作的整个速度取决于磁盘子系统及其算法hinges on disk subsystem and its algorithm。

操作系统活动：①free-space空余空间管理；②存储分配storage allocation；③硬盘调度disk scheduling。

一些存储不需要快速：①三级存储tertiary包括光存储optical storage，磁带magnetic tape；②仍需管理；③WORM(write-once, read-many-times),和RW(read-write)的区别。

##### 7.I/O子系统subsystem

操作系统的一个目的是向用户隐藏硬件设备的特性hide prculiarties使得易于使用和编程。I/O子系统负责：①I/O内存的管理包括缓冲buffering(在传输时暂时存储数据)，②缓存caching(将部分数据存储在更快的存储中以提高性能)，③假脱机spooling(一个作业的输出与其他作业的输入重叠overlapping)

特定硬件设备的通用设备驱动程序接口和驱动程序

##### 8.保护和安全protection and security

保护--操作系统定义的用来控制进程或用户访问资源的机制；安全security--保护系统免受内部和外部共计attacks：范围很广huge range, 包含决绝服务denial-of-service, 蠕虫worms，病毒viruses，身份盗窃identity theft，服务盗窃service theft.

系统首先区分用户以确定谁可以做什么：①用户身份identity(user ID, 安全ID) 包括名称和每个用户一个；②用户ID然后和所有文件和用户决定访问控制权的进程关联；③组标识符group ID，允许用户定义一组用户并管理空间controls, 然后与每个进程关联；④特权升级privilege escalation允许用户更改为具有更多权限的有效ID。

##### 9.计算环境computing environments

传统计算机：①随着时间推移而模糊blurring over time；②办公环境：PC连接到网络，终端连接到主机或小型机；现在门户portals允许网络和远程系统访问相同的资源。家庭网络较为单一。

①Client-Server Computing客户端-服务端计算：

②Peer-to-Peer Computing：分布式系统的一种，不区分客户端和服务端

③Web-Based Computing：更多设备联网以允许网络访问









