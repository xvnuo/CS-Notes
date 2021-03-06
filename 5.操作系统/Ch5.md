# Ch5 CPU scheduling

## 基本概念basic concepts

引入多程序涉及multi programming目的是提高计算机资源利用率，尤其是获得最大CPU利用率utilization。

CPU密集--I/O密集的循环。

进程的执行呈现出CPU运行burst和I/O等待wait的交替循环。CPU运行和I/O等待的交替循环：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211184443233.png" alt="image-20201211184443233" style="zoom:67%;" />

##### 1.CPU调度器 scheduler

CPU调度器注重业务逻辑--按照一定的调度算法来。它从内存中准备执行ready to execute的进程中进行选择select from，并将CPU分配给选中的进程来执行。

CPU scheduling调度可能在以下几种情况下发生：①进程从running状态切换到waiting状态；②从running状态切换到ready状态；③从waiting切换到ready状态；④终止状态terminates。

非抢占式nonpreemptive：进程自愿交出CPU资源引起的调度；抢占式preemptive：因为优先级更高抢占或被抢占执行顺序的过程。

①和④的调度都是非抢占的nonpreemptive: ①该进程需要资源，自愿处于waiting状态；④则主动终止。②③是抢占式preemptive：②是时间片被抢走了，例如有一个优先级更高的进程进入就绪队列；③是需要的资源到了，优先级比较高，抢占了别人的时间片。

##### 2.CPU派发器dispatcher

CPU分发器/派发器dispatcher相对于CPU调度器scheduler的区别是，它轻业务逻辑甚至没有业务逻辑，是纯粹的转发派发。它做的事情不涉及到决策，是固定的事情，所以它主要是对短期调度涉及的进程进行调度，包括：①上下文进程切换switching context；②切换到uesr模式；③跳到用户程序中的适当位置重新启动该程序。

分配延迟dispatch latency：调度器停止一个进程开始另一个进程所需时间。调度延迟的冲突conflicts阶段包含两个部分：①在内核中运行的任何进程的抢占preemption of any process running in the kernel; ②从旧的进程中为新进程释放资源。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211185333341.png" alt="image-20201211185333341" style="zoom:67%;" />



##### 3.CPU调度器追求的指标scheduling criteria

就是衡量调度过程的性能的一些指标：①CPU利用率utilization：尽可能使CPU一直处于不断busy运行的状态；②吞吐量throughput：单位时间内完成执行的进程数；③周转时间turnaround time：执行完某一个进程所耗用的CPU累计时间；④waiting time等待时间：某一进程在ready queue中等待的累计时间；⑤响应时间response time：某一个进程从发出调度请求request was submitted到它得到CPU调度器响应first response，所经历的时间，到调度开始响应就算完，不需要完成响应。

最优化原则optimization criteria: ①最大CPU利用率max CPU utilization；②最大吞吐量max throughput；③最小周转时间turnaround time；④最小等待时间min waiting time；⑤最小响应时间min response time。

## 调度算法scheduling algorithm

##### 1.先来先服务first-come, first-served, FCFS scheduling

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211192657807.png" alt="image-20201211192657807" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211192757688.png" alt="image-20201211192757688" style="zoom:67%;" />

先来先被CPU执行，按照队列的顺序，先进先执行先出。平均等待时间不是最优的，但是CPU利用率为100%。短进程先于长进程到来平均等待时间改善很多。

##### 2.最短有限算法Shortest-Job-First, SJF

进入就绪队列ready queue的进程预告需要多长CPU时间才能完成本次执行。选取就绪队列中，“需要CPU时间”最短的进程先执行。

抢占式preemptive：如果一个新的进程进入ready queue，且执行时间CPU burst time比当前正在执行的进程的剩余执行时间短，那么就可以抢占过来运行新进程。抢占式的SJF算法也叫做SRTF，shortest-remaining-time-first. 这种类型拥有最短的平均等待时间。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211193845789.png" alt="image-20201211193845789" style="zoom:67%;" />

非抢占式的non-preemptive：一旦CPU分配给了某个进程，就不能抢过来，除非该进程主动放弃CPU(CPU burst cycle结束或者该进程转去做I/O操作)

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211193824992.png" alt="image-20201211193824992" style="zoom:67%;" />

算法问题：进程没办法准确预报自己花费的CPU时间，执行时间可能和资源和实际的情况有关，所以只能停留在理论上。

进入就绪队列ready queue的进程如何预估“CPU 时间”？不可能准确的预测，只能根据过去的CPU burst cycles拟合，如指数平均exponential average思想：下一次(第n+1次)执行时的CPU时间=第n次实际执行时间*a+(1-a)*第n次预测的执行时间。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211195327749.png" alt="image-20201211195327749" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211195350452.png" alt="image-20201211195350452" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211195706753.png" alt="image-20201211195706753" style="zoom:67%;" />

非抢占式情况下，SJF调度的平均等待时间也是最优的optimal。

##### 3.优先权法priority scheduling

每个进程都有一个优先数priority number，通常为整型数字，选取ready queue中优先权最高的进程先执行(最小整数=最高优先级)，包括抢占式和非抢占式两种方式。当优先权定义为进程需要的CPU时间时，SJF就等同于优先权法。

算法问题：饿死starvation，低优先级的进程可能一直不会执行；

解决办法：aging，随着时间不断提升进程的优先级，进程变老的同时优先级提升了，总会轮得到的。所以等待在就绪队列中的进程它的优先权单调递增。

##### 4.轮转法round robin, RR

每个就绪进程获得一小段CPU时间(时间片，time quantum)，通常为10ms~100ms；时间片用完后这个进程被迫交出CPU，重新挂回到就绪队列。如果进程在时间片用完之前他的burst cycle结束，也要主动交出CPU，假设n个就绪进程，时间片为q，每个就绪进程得到1/n的CPU时间，任何就绪进程最多等待(n-1)/q 

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211201216552.png" alt="image-20201211201216552" style="zoom:67%;" />

性能分析：①优势：任何一个进程的等待时间是有上限的。②平均周转时间通常长于SJF；③但响应时间优于SJF；④分配给不同的进程时每次切换都要做一次上下文切换context switch，会影响效率。q较大时完成一个进程不需要切换，就类似FIFO；q较小时，上下文切换开销太大，q必须远远大于上下文切换时间。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211201431830.png" alt="image-20201211201431830" style="zoom:67%;" />

##### 5.多层队列算法multilevel queue

把ready queue拆分成几个队列，如要求交互(interactive)的进程在前台队列foreground，可以批处理(batch)的进程在后台队列background。每个队列可以有自己的调度算法；如前台就绪队列--RR算法(响应时间短)；后台就绪队列--FCFS。不同队列之间可以有不同的优先级。前台侧重于响应时间使用轮转法，后台对响应时间步敏感使用先来先服务算法。系统调度要实时响应，交互要及时，批处理和交互没啥关系，所以响应时间不那么重要。

不同队列之间的优先级：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211202320366.png" alt="image-20201211202320366" style="zoom:67%;" />

CPU如何在队列间进行分配？①固定优先权法fixed priority scheduling，如先前台队列再后台队列；②时间片方法time slice，如80%CPU时间给前台队列，20%给后台队列。现在的时间片的竞争主要是队列间的竞争。

##### 6.多层反馈队列法multilevel feedback queue

类似多层队列算法，但是进程可以在ready queue之间迁移，这主要考虑到进程在不同情况、任务下对实时性的需求是有变化的，有时候注重交互，有时候重计算。多层反馈队列调度器由以下参数定义：①队列个数；②每个队列的调度算法；③决定何时将就绪进程升级upgrade到高层次队列的算法；④决定何时将就绪进程降级demote到低层次队列的算法；⑤决定当一个就绪进程就如就绪队列时去哪层的算法which queue a process will enter when the process needs service。

实例：三个队列，Q0(RR, q=8ms), Q1(RR, q=16ms), Q2(FCFS); 调度过程：①一个就绪进程进入Q0层，当它分配到CPU时可以执行8ms,如果8ms之后没有执行完毕则迁移到Q1层，否则它执行完毕就离开就绪队列该干嘛干嘛。②在Q1层，当它分配到CPU可执行16ms,如果它16ms后没有执行完毕则迁移到Q2，否则它执行完就离开就绪队列该干嘛干嘛。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211204726724.png" alt="image-20201211204726724" style="zoom:67%;" />

##### 7.实时调度real-time scheduling

希望调度要很及时，但是不同任务的及时性的要求不同。但是这一点很难，特别是通用性的操作系统几乎不可能实现及时性，因为不同进程的要求不一样用同一的调度方法不可能完全何时。

①硬实时系统hard real-time：调度机制能够确保一个关键任务critical task在给定时间点guaranteed amount of time前完成；②软实时计算soft real-time computing：调度机制尽量给与关键任务critical processes较高优先级，尽量在预定时间点前完成。

①在嵌入式系统重使用硬实时调度，也就是时间结点是明确的，如果没有按时完成就作废missing a deadline is a total system failure，②firm：不经常的截至日期是可以容忍的，但可能会降低degrade系统的服务质量，结果的效用在截至日期之后为零。③soft: 结果的有用性在截至日期之后下降，从而降低了系统的服务质量。通用操作系统一般是软实时调度，只能尽量满足。

##### 8.多处理器调度multiple-processor scheduling

多个CPU的调度更加复杂。多处理器重的同质处理器homogeneous,负载平衡load banlancing, 非对称多处理器asymmetric multiprocessing--只有一个处理器访问数据结构，减轻了数据共享的需要；其他处理去只执行用户代码user code. 对称多处理Symmetric multiprocessing, SMP, 每个处理器都是自调度的self-scheduling。多个处理器可能访问和更新一个通用的数据结构。

## 线程调度thread scheduling

本地调度local scheduling/进程保留范围process contention scope--线程库thread library决定将哪个线程which thread放到可用的LWP上。

全局调度Global Scheduling/系统维护范围 System Contention Scope--内核kernel如何决定下一步运行哪个内核线程kernel thread

## 操作系统的例子Operating System Examples

##### 1.Solaris Scheduling

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211213437407.png" alt="image-20201211213437407" style="zoom:67%;" />

##### 2.Windows XP scheduling

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211213518639.png" alt="image-20201211213518639" style="zoom:67%;" />

##### 3.Linux Scheduling

两种算法：time-sharing和real-time

time-sharing: 是优先的基于信用的prioritized credit-based，最多credits的进程优先执行。当计时器中断timer interrupt发生时，信用减少credits subtracted。当credit=0时，该进程被另一个进程替换(抢占)。当所有进程的credit都=0时，按优先权和历史记录等(factors including priority and history)重新计分recrediting。

real-time: 软实时soft real-time, FCFS和RR结合，最高优先级总是首先被执行:   时间片到了之后引起中断响应，中断时从当前执行的进程的PCB拿出来，访问counter并减一，如果不等于0，该进程就继续响应，如果等于0，就重新设置counter(不设置统一的值，可以按照优先级和历史记录给每个进程设置一个值)，调用scheduler基于优先级重新调度。

优先级和时间片长度的关系：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211214959949.png" alt="image-20201211214959949" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211215101843.png" alt="image-20201211215101843" style="zoom:67%;" />

## 调度算法的评估Algorithm Evaluation

由于实际情况是非常复杂的，要确定和理论计算非常难，所以确定模型法也一般不靠谱：

①确定模型法deterministic modeling：采用实现设定的特定负荷，计算在给定负荷下每个算法的性能。

②排队模型queueing models：先建立一个模型来模拟实际情况对算法进行测试，但是这对于模型的准确性就有了要求，要求其能尽可能反映实际使用场景，但是模型的准确率也很难达到很高。

③仿真simulations：采集实际情况的数据，临时搭建平台进行测试，采集指标。这种方法效果不错，理论难度较低，是偏工程性的方法。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201211215810171.png" alt="image-20201211215810171" style="zoom:67%;" />



























