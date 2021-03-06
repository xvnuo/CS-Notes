# Ch2: Operating-System Structure

## 操作系统服务Operating System Service

一组操作系统服务提供对用户有帮助的功能：①用户界面user interface, UI,几乎所有操作系统都有，和命令行(Command Line, CLI),图形用户界面(Graphics User Interface,GUI)和批处理(Batch)并列；②程序执行program execution，系统必须能够将程序加载load到内存并运行该程序、结束执行，无论是正常的还是不正常的(error)；③I/O操作：运行程序可能需要I/O，这可能涉及文件或I/O设备；④文件系统操作File-system manipulation: 程序需要读写文件和目录，创建或删除，搜索，列举文件信息，和文件权限管理permission management。⑤通讯交流communication：同一台计算机或网络上的计算机之间的进程可能需要交换信息，通信可能通过共享内存shared memory或由OS移动的数据包packet传递。⑥错误检错error detection: OS需要不断检测出可能出现的错误，错误可能发生在CPU或内存硬件、I/O设备中。对每种类型的错误，OS应该采取何时措施确保正确和一致的计算。调试debugging可以极大提高用户和程序员有效使用系统的能力。

## 用户与操作系统交互User Operating System Interface

命令行界面(CLI, command line interface)允许直接命令条目direct command entry: ①有时在内核kernel实现，由系统程序system program实现。②优势由多种类型muliple flavors实现--shell;③主要是从用户获取命令fetch command并执行它：优势命令内置built-in (DOS)，优有时只是程序名称(Unix)，如果是后者，添加新功能不需要shell修改。

用户友好的桌面隐喻界面user-friendly desktop metaphor interface：①通常使用鼠标键盘和显示器交互；②用图标icons表示文件程序和行为操作等；③不同鼠标按钮导致不同的操作，提供信息选项执行打开目录等功能。

许多系统同时使用CLI和GUI界面：①微软windows是带有CLI命令shell的GUI；②苹果是"Aqua" GUI界面，带有UNIX kernel和shell命令；③Solaris是CLI带有可选GUI接口的(Java desktop, KDE).

## System Calls

##### 1.概念

操作系统提供的服务的编程接口programming interface；通常由高级语言(C或C++)编写；主要通过高级应用程序接口(API，application program interface)访问，而不是直接的系统调用system call使用；最常见的三个API是Windows的Win32API\ 基于POSIX系统的POSIX API(几乎包括UNIX, Linux和MaxOS的所有版本)以及Java虚拟机的Java API(JVM).

##### 2.系统调用System call的实现

通常一个数字和一个系统调用相关联，系统调用接口维护根据这些数字建立索引的系统调用的信息表。系统调用接口在OS内核中调用预期的系统调用invoke intended system call并返回系统调用的状态和其他的返回值。调用者caller不需要知道系统调用是如何实现的，只需要遵循obey API并了解OS将作为结果调用做什么what OS can do as a result call。API向程序员隐藏OS接口的大部分细节。运行时支持的库来管理run-time support library(内置在包括编译器的库中的一组函数,如C库)。

API--System call--OS，三者的关系：

![image-20201208104638262](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208104638262.png)

例如标准的C库：用户态下C程序使用printf()库的调用，会调用kernel态下的系统调用wirte()

![image-20201208105012804](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208105012804.png)

##### 3.系统调用的参数传递system call parameter passing

通常需要更多的信息/参数，而不是简单的标识所需的系统调用：具体的信息类型和数量因操作系统和系统调用类型而异。

将参数传递给操作系统的三种方法：①最简单的寄存器传递：通过寄存器传递参数，某些情况下，传递的参数数量可能比寄存器数量还多；②块方法：参数存储在block\table\或内存中，该block的地址作为参数通过寄存器传递；③栈方法：程序将参数防止或pushed到栈上，操作系统使用时从栈上弹出pop；块和栈的方法不限制传递的参数的数量和长度。

通过table传递参数：用户态变量-->操作系统内核系统调用需要的参数

![image-20201208105702734](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208105702734.png)

##### 4.系统调用的类型Types of System Calls

①进程控制process control

②文件管理file management

③设备管理device management

④信息维护information maintenance 

⑤交流通讯communication

## 系统程序System programs

系统程序为程序开发和执行提供方便的环境，其中的一些只是系统调用的用户界面user interface，另一些则更复杂。可分为：①文件管理file manipulation：创建、删除、复制、重名了、打印、转储dump、列表等对文件和目录的操作；②状态信息status information：有些询问系统提供日期时间内存数量磁盘空间等信息，有些提供详细性能信息如日志记录和调试信息，通常这些程序将输出格式化并打印到终端或其他输出设备，有些系统实现一个注册表registry，用于存储和检错retrieve配置信息。③文件修改file modification：文件编辑器text editors创建和修改文件，搜索文件内容或执行文本转换的特殊命令等。④交流通讯：提供在进程用户和计算机系统之间创建虚拟连接的机制，允许用户将消息发送到彼此的屏幕，浏览网页，发送邮件等。⑤应用程序application programs⑥编程语言支持programming language support：如编译器compilers，汇编器assembler，调试器debuggers和解释器interpreters等；⑦程序装在和执行program loading and execution：绝对加载器loaders, 可重定位加载器、连接编辑器和覆盖加载器、高级语言调试系统等。大多数用户对操作系统的试图是由系统程序定义的，而不是实际的系统调用。

## 操作系统设计和实现OS design and implementation

不同操作系统内部结构很不同。从定义goals和规范specifications开始；受硬件和系统类型的影响。用户目标user goals：便于使用和学习可靠安全快速；系统目标system goals：易于设计、实现和维护，灵活可靠，无错，高效。

##### 1.策略和机制的分离原则separate

policy：what will be done?  Mechanism: how to do it? 政策和机制不同意义。机制决定如何做某种事情，策略决定要做什么。机制和策略分离可以在策略以后会改变的情况下实现最大限度的灵活。

例如，使用卡键card-keys访问锁定的门locked doors：机制对应为磁读卡器，远程控制所与安全服务器的连接；策略对应为应该允许哪些人进入，在哪些时间可能进入。

##### 2.操作系统的结构operating system structure

①简单的结构如MS-DOS：在最小的空间中提供最多的功能；不分模块，虽然它有一些结构但是它的接口和功能没有很好的分离。层次结构如下：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208113208434.png" alt="image-20201208113208434" style="zoom:67%;" />

操作系统分成若干层layers，每层都建在下一层的上部。底层是硬件，最高层是用户界面user interface。模块化modularity通过层结构实现，这样每层只使用较低层的函数操作和服务。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208114920463.png" alt="image-20201208114920463" style="zoom:67%;" />

②UNIX操作系统--受硬件功能限制，最初的UNIX操作系统结构有限，可由两个分离的部分组成：System programs和kernel(包括系统调用界面下面和物理硬件之上的所有内容)，提供文件系统，CPU调度，内存管理和其他操作系统功能，大量功能在同一个层次。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208115502779.png" alt="image-20201208115502779" style="zoom:67%;" />

③微软系统MicroKernel System Structure--从内核大量移动到用户空间；在用户模块之间使用消息传递进行通信。更易将操作系统移植到新的体系结构；更可靠(在内核模式下运行的代码较少)。但是用户空间对内核空间通信的性能开销较大。

##### 3.Modules

大多数现代操作系统都实现内核模块化kernel modules. 使用面向对象的方法，每个核心组件都是分开的；每个模块都通过已知的接口和其他交流；模块互相调用而不是消息传递。每个模块都可以根据需要在内核中加载。类似层但更灵活。

eg: Solaris Modules Approach

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208120117952.png" alt="image-20201208120117952" style="zoom:67%;" />

## 虚拟机Virtual Machine

虚拟机对器逻辑结论logical conclusion采取分层放大layered approach，它对待硬件和操作系统内核就好像它们都是硬件一样。虚拟机提供与第层裸硬件bare hardware相同的接口。操作系统创建多个进程并产生每个进程都在自己的处理器上使用自己的(虚拟)内存内存执行的错觉。

物理计算机的资源被共享来创建虚拟机：①CPU调度可以创建产生用户拥有自己处理机的样子；②spooling和文件系统可以提供虚拟机读卡器和虚拟路线打印机；③正常的用户分时终端作为虚拟机操作员的控制台。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208120630995.png" alt="image-20201208120630995" style="zoom:67%;" />

虚拟机概念提供了系统资源的完整保护，因为每个虚拟机都与所有其他虚拟机隔离。 然而，这种孤立不允许直接分享资源。虚拟机器系统是操作系统研发的理想工具。系统开发是在虚拟机上进行的，而不是在物理机器上进行的，因此不会破坏正常的系统操作。虚拟机概念很难实现，因为需要努力向底层机器提供精确的副本。（例如，虚拟用户模式和内核模式）

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208120817478.png" alt="image-20201208120817478" style="zoom: 67%;" />

## 操作系统的启动Operation System Generation

操作系统的设计是为了在任何一类机器上运行；系统必须为每个特定的计算机站点computer site进行配置configured。SYSGEN程序获取有关硬件系统特定配置的信息。启动booting-通过加载内核kernel启动计算机。启动程序Booting program是存储在ROM中的代码，这些代码能够定位内核，加载内核到内存中，并开始执行

操作系统必须是可供硬件访问的，以便硬件可以启动它。小块代码-引导程序bootstrap porgram(也叫引导加载程序Bootstrap loader)，定位内核，将其加载到内存中，并启动它。有时是两步过程，在固定位置的引导块boot block先加载引导加载程序boootstrap loader,然后引导程序再定位内核加载内核到内存中并开始启动。当在系统上初始化电源power时，执行从固定的内存位置开始，固件firmware用于保存初始引导boot code代码。



