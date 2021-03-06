# Ch3: Process

## 进程的概念Process Concept

##### 1.进程结构

操作系统执行各种程序①批处理系统，即作业Batch system--jobs和②时间共享系统，即用户程序或任务time-shared systems--user programs or tasks. job==process. 

进程是顺序执行的程序program。一个进程包含：①文本部分(text section, code)，②程序计数器program counter，③栈stack(存放函数参数parameters，局部变量local vars和返回值return)，④数据部分data section(全局变量等global vars)，⑤堆heap(动态分配的内存dynamically allocated memory)。

内存中的进程结构：

![image-20201208132708720](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208132708720.png)

堆结构：堆是一个完全二叉树complete binary tree，除了叶子结点，所有层都是慢的，叶子结点从左往右本填充。结点的值应该比它的所有孩子都小/大。

栈结构：栈指针通常以硬件寄存器的形式指向栈上最近引用的位置。push操作，数据被放到栈顶，堆栈指针中的地址根据数据项的大小进行调整；pop操作...

##### 2.进程的状态process state

进程状态随着进程执行的过程而变化：①new: 进程正在被创建；②running：进程中的指令正在被执行；③waiting：进程正在等待或被一些事件event所阻塞；④ready：进程正在等待被分配给一个处理器处理；⑤terminated：完成执行。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208133338758.png" alt="image-20201208133338758" style="zoom:80%;" />

##### 3.进程控制块process control block, PCB

存储与进程相关的信息：①进程状态process state；②程序计数器PC，记录下一个该执行的指令位置；③CPU寄存器的内容registers；④CPU的调度信息scheduling information，记录优先级；⑤内存管理信息memory-management information；⑥记账accounting信息，记录CPU/time used,时间限制和进程号process number.⑦I/O的状态信息status information：分配的设备和打开的文件等。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208133842188.png" alt="image-20201208133842188" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208133933101.png" alt="image-20201208133933101" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208134234183.png" alt="image-20201208134234183" style="zoom:67%;" />

## 进程的调度process Scheduling

##### 1.进程的调度队列process scheduling queues

①Job queue: 存放系统中所有的进程；②ready queue: 驻留在主存中的所有进程的集合，准备就绪或等待执行ready and waiting to execute；③device queues：等待I/O设备的进程集合。进程在各种不同的队列之间迁移migrate。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208134627088.png" alt="image-20201208134627088" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208134724377.png" alt="image-20201208134724377" style="zoom:67%;" />

##### 2.调度者schedulers

是一个程序。分成两类：①长期调度long-term scheduler，即job scheduler, 决定哪个进程应该改放入内存(进入 ready queue的过程)；②短期调度short-term scheduler 即CPU scheduler, 决定哪个进程下一步应该被分配CPU执行。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208135108450.png" alt="image-20201208135108450" style="zoom:67%;" />

还有中期调度medium-term scheduling. 用来将进程交换出来swap out:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208135257875.png" alt="image-20201208135257875" style="zoom:67%;" />

短期调度被频繁调用，milliseconds快速；长期进程较为频繁但略慢seconds,minutes. 长期调度控制着the degree of multiprogramming. 

进程可分为两类：①I/O-bound process--花更多的时间做I/O读写而不是计算 CPU burst更短；②CPU-bound process--花更多的时间做计算computations，CPU burst更长。需要将两者进行良好的组合。

##### 3.上下文切换context switch

当CPU从当前进程切换到另一进程时，系统必须保存该进程的状态并装载进新进程的状态saved state。上下文切换的开销是时间time is overhead. 系统在切换时不能做什么实际性的工作--通常耗费milliseconds.时间开销取决于硬件支持。

## 进程的操作operations on process

##### 1.创建进程

父进程创建子进程，子进程创建其他进程，形成进程树。

资源共享的3种模式--①父进程和子进程共享所有资源；②子进程分享父进程资源的子集；③父进程和子进程不共享任何资源。

执行的两种模式：①父子进程并发执行；②父进程等待子进程执行结束后再开始执行。

地址空间address space：①子进程是父进程的复制品，它和父进程有同样的程序和数据；②子进程载入另一个新的程序 

##### 2.UNIX的进程创建例子

UNIX中每个进程都用唯一一个整型进程标识符来标识，系统调用fork()创建一个新进程；新进程的地址空间复制了原进程的地址空间，子进程继承了父亲的权限、调度属性以及某些资源。这种机制允许父子进程轻松通信。<font color="blue">这两个进程都继续执行系统调用fork()之后的指令，但对于新的子进程，系统调用fork()的返回值为0；对于父进程返回值为子进程的标识符，非零。有个进程使用系统调用 exec()，以用新程序来取代进程的内存空间。</font>

系统调用 exec() 加载二进制文件到内存中（破坏了包含系统调用 exec() 的原来程序的内存内容），并开始执行。采用这种方式，这两个进程能相互通信，并能按各自方法运行。父进程能够创建更多子进程，或者如果在子进程运行时没有什么可做，那么它采用系统调用 wait() 把自己移出就绪队列，直到子进程终止。因为调用 exec() 用新程序覆盖了进程的地址空间，所以调用 exec() 除非出现错误，不会返回控制。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208141107431.png" alt="image-20201208141107431" style="zoom:67%;" />

```c
int main()
{
    pid_t pid; //存储父子的进程号
    pid=fork();//fork产生一个新的子进程，父的pid非零，子pid为0
    if(pid<0){ //pid<0说明子进程创建失败
        fprintf(stderr,"Fork Failed");
        exit(-1);
    }
    else if(pid==0) execlp("/bin/ls","ls",NULL);//说明是子进程
    else{//pid>0父进程要执行的东西
        wait(NULL);
        printf("Child Complete");
        exit(0);
    }
}
```

没有什么可以阻止子进程不调用 exec()，而是继续作为父进程的副本来执行。在这种情况下，父进程和子进程会并发执行，并采用同样的代码指令。由于子进程是父进程的一个副本，这两个进程都有各自的数据副本。

fork()函数的使用用例：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208142802410.png" alt="image-20201208142802410" style="zoom:67%;" />

##### 3.进程终止

进程执行完最后一条语句并要求操作系统调用exit()删除它，进程终止.进程可以返回状态值(通常为整数pid)到父进程(通过系统调用wait)。所有进程的资源如物理和虚拟内存、打开文件和I/O缓冲区等被操作系统释放和回收deallocated。

父进程在某些情况下可能终止子进程的执行abort：①子进程使用了超过它所分配的资源；②分配给子进程的task不再被需要了；③父进程正在退出，而且操作系统不允许没有父进程的子进程继续执行。

有些系统不允许子进程在父进程已终止的情况下存在。对于这类系统，如果一个进程终止（正常或不正常），那么它的所有子进程也应终止。这种现象，称为级联终止cascading termination，通常由操作系统来启动。其他操作系统情况下，可能子进程可以以孤儿orphaned的身份存在。

## 协作进程cooperating process

独立independent进程不能影响或受到另一个进程执行的影响。进程之间的协作可能会影响或受到另一个进程的影响。进程协作的优点：①信息共享information sharing；②计算速度提升computation speed-up(Multiple CPUs)；③模块化Modularity；④方便性convenience

##### 1.生产者-消费者问题Producer-Consumer problem

生产者进程producer process产生的信息被消费者进程consumer process消耗 无界缓冲区unbounded-buffer对缓冲区的大小没有实际的限制。如果没有新的item 消费者必须等待。有界缓冲区bounded-buffer假设有一个固定的缓冲区大小，如果缓冲区已满，生产者必须等待。

对于有界缓冲区，假设存在一个round table，环形表，in和out分别为头指针和尾部指针，则实现如下：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208145456461.png" alt="image-20201208145456461" style="zoom:50%;" />

```c
//缓冲区存放的数据类型item
#define BUFFER_SIZE 10
typedef struct{
    ...
}item;
int buffer[BUFFER_SIZE];//缓冲区
int in=0,out=0;//in随着插入逐渐后移，out随着删除也逐渐后移

//插入过程,(in+1)%BUFFER_SIZE==out说明缓冲区已经满了
while(true){
    //产生一个item并插入round table中，in后移
    while(((in+1)%BUFFER_SIZE ==out)) ;//表满了，等待消耗
    buffer[in]=item;
    in=(in+1)%BUFFER_SIZE;
}

//删除过程，in==out说明表内为空
while(true){
    while(in==out) ;//表空，等待生产者生产item
    //从缓冲区移除一个item
    item=buffer[out];
    out=(out+1)%BUFFER_SIZE;
    return item;
}
```

## 进程间的通信interprocess communication，IPC

##### 1.通信方式

进程通信和同步行动的机制。IPC的模型：message passing和shared memory。消息系统message system：进程之间相互通信，而不依赖于共享变量shared vars. IPC提供两个操作：①send(message)--message大小固定或可变；②receive(message)：接收消息。

如果P和Q进程要进行交流，需要：①在进程之间建立连接communication link；②通过send/receive交换信息。实现通信连接communication link：①物理上--共享内存，硬件总线等；②逻辑上--逻辑性质等.

##### 2.实现细节

links如何建立？一个link可以和多个进程相互连接吗？每对进程之间可以有多少个link? link的容量？ 连接的link是可承载信息的容量是否可变？有向传输or无向

##### 3.通讯模型communication models

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208151549351.png" alt="image-20201208151549351" style="zoom:67%;" />

直接通讯direct communication：进程必须明确地彼此命名，两个操作①发送消息给P--send(P, message)；②从Q处接收消息receive(Q, message)。通讯link的性质：①连接自动建立；每个pair进程仅有一个link；③每个link仅和一对进程相关联；④link可能是双向bidirectional，也可能是单向的unidirectional。

间接通讯Indirect communication: 消息是从mailboxes(即端口ports)定向收发: ①每个邮箱都有唯一的ID；②进程只有在他们共享邮箱时才能通信。连接的性质：①只有在进程共享一个公共邮箱时才能建立连接；②连接可能与许多进程关联；③每对进程可能共享好几个通信连接；④连接可能是单向或双向的。常见操作：①建立新的mailbox；②通过mailbox收发信息；③销毁mailbox。原语primitives定义为：①向mailbox A发消息--send(A, message)；②从mailBox A接收消息--receive(A, message)。

问：若进程P1,P2,P3共享mailBox A, P1sends, P2/P3 receive, 谁能收到消息？3种解决方案😚：①允许一个 link与至多与两个进程关联；②一次只允许一个进程执行接收操作；③允许系统任意选择接收人，发件人被告知接收人是谁。

##### 4.同步Synchronization

消息传递可能是阻塞blocking或非阻塞non-blocking的。

阻塞被认为是同步的synchronous. 发送端有发送块send block知道message被接收；接收端有接收块receiver block直到获得消息。

非阻塞被认为是异步asynchronous，发送方发送消息and continue;接收方接收到消息或接收到NULL。

##### 5.缓存 Buffering

消息的队列连接到link，有三种实现方式：

①零容量Zero capacity--0 message，发送方必须等待接收方(rendezvous会和) ②有限容量bounded capacity--finite length of n messages, 如果连接满发送者必须等待有接收者接收产生空余才能再次开始发送。③Unbounded capacity--infinite length: 无约束容量，发送者从来不等待。

## Client-Server System通讯方式

1.Socket

socket是通讯的端点。连接IP地址和端口port, concateenation.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201208161355317.png" alt="image-20201208161355317" style="zoom:67%;" />

2.Remote Procedure Calls

远程过程调用(RPC)抽象网络系统上进程之间的过程调用。Stubs-客户端代理服务器上的实际过程。 客户端存根定位服务器并编组参数。 服务器端存根接收到此消息，则解压缩的参数，并在服务器上执行此过程。

3.Remote Method Invocation(Java)











