# Ch6 Process Synchronization

进程同步存在问题，原因就是一个CPU要为两个以上的进程服务，而这其实是现在的操作系统也没有完美解决

## 背景

##### 1.背景

并发访问共享数据可能导致数据不一致,保持数据一致性需要机制来确保协作进程cooperation processes的有序执行。以生产者-消费者模型为例，设计一个整型变量count,记录缓冲区被占用的单元总数，count初始值为0，当生产者进程注入一个单元数据时，count+1；当消费者进程消费掉一个单元数据时count-1。

如果不加处理的话，就会出现问题：假设两个进程要访问同一个资源，由于CPU调度具有一定的随机性，而先访问的进程会对资源进行修改，这就使得进程对资源的访问结果具有一定的随机性，这显然是不可接受的。

##### 2.生产者消费者模型

一种解法--共享变量shared data描述有界缓冲区bounded buffer

<img src="https://gitee.com/jiadingMayun/Pic/raw/master/img/image-20200205101705165.png" alt="image-20200205101705165.png" style="zoom: 50%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229203029602.png" alt="image-20201229203029602" style="zoom:80%;" />

生产者：若缓存满了，等待至有空余地方，然后装入产生的数据单元。count++

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229203043863.png" alt="image-20201229203043863" style="zoom:80%;" />

消费者，若缓存空，则等待至有东西了，拿走该数据单元，然后count--

其中的2条语句count++, count--必须独立不受干扰的执行。原子操作atomic operation，要求该操作系统完整地一次性完成，不允许中间被打断。

如果这两个操作在不被打断的情况下执行，那么就不会有同步问题。也就是说，涉及到共享资源的写时，操作需要是独立不受干扰地执行的。但是要实现原子操作不是直观看上去那么简单的，因为它在实际执行时可能根本不是一条语句，而是多条（例如像例子中的这种情况，不是所有的CPU都有直接对内存变量进行修改的指令，许多CPU指令集，特别是精简指令集，就像下面展示的那样，还需要借助寄存器）

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229203830050.png" alt="image-20201229203830050" style="zoom:80%;" />

**注意，单条机器指令在执行的过程中是不会被中断的**，这是因为此时是关中断了的。

*除了指令集本身的限制外，还可能有编译程序的限制*。由于编译器要在不同平台上提供服务，虽然每个平台都是单独设计的，但是不一定能针对该平台充分优化。例如上图这种情况，如果编译器优化不到位，在所有平台遇到++都翻译为上述三条指令，那当然在各个平台都可以运行，虽然不是最优的。这也使得同步问题更加地突出。

<img src="https://gitee.com/jiadingMayun/Pic/raw/master/img/image-20200205105347534.png" alt="image-20200205105347534.png" style="zoom:67%;" />

为了避免race conditions，并发进程必须同步synchronized。

## 临界区问题The Critical-Section Problem

##### 1.基本概念

假设n个进程竞相访问共享数据：每个进程存在一段代码，称为临界区critical section，进程就是通过这段代码访问了共享数据shared data。其他代码段没有访问共享数据，这n个进程中，至少存在1个以上的进程甚至修改了数据。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229204706703.png" alt="image-20201229204706703" style="zoom:67%;" />

注意临界区这个词是形容进程代码的，并且不包括所有进程只读不写的情况。临界区是和进程相关，什么是临界区问题呢？

临界区问题：怎样确保，当有一个进程i正在自己的临界区执行时，没有任何其他进程j也在它的进程j中的临界区中执行该临界区。

##### 2.临界区的解决方案必须满足的3个条件

①互斥mutual exclusion--如果进程Pi正在临界区执行，那么其他任何进程都不允许在它们各自的临界区中。

②空闲让进progress--如果没有进程处于它的临界区，且某些进程申请进入其临界区，那么，只有那些不在reminder sections的进程，才能参与能否进入临界区的选举，且这个选举不允许无限期退出idefinitely。

entry section和exit section是为了解决临界区问题而加入的、原本代码中没有的代码块，而**临界区问题的解决方案也就是确定这两个区域的代码内容的问题**

注意，当我们讨论一个资源的时候，另外的资源代码都算成remainder section。也就是说，**选举必须是直接相关的进程才能做的**

③有限等待bounded waiting--某一个进程从其提出请求，到它获准进入临界区的这段时间里，其他进程进入它们的临界区的次数存在上界：假设进程各自都在继续执行，不考虑N个进程之间的相互执行速度。

## Peterson's Solution

是两个进程的解决方法。假设进程Pi和Pj.

假设LOAD和STORE指令是原子性的，不能被中断。定义两个进程的共享数据：

> int turn;//trun只能取i或j，指示两个进程中的哪一个可以进入临界区
>
> boolean flag[2];//flag[i]=true，表示进程Pi申请进入临界区。

彼得孙算法定义进程Pi:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229210142655.png" alt="image-20201229210142655" style="zoom:50%;" />

flag[i]=true，说明i提出请求了，turn=j，说明运行完之后轮到j。while内意思是说只有既轮到j了，同时j又申请的情况下，i才会做空操作，等待.否则就进入临界区执行操作。

<img src="https://www.z4a.net/images/2020/02/05/image-20200205153443504.png" alt="image-20200205153443504.png" style="zoom:50%;" />

## 硬件同步机制Synchronization Hardware

CPU制造者为临界区问题提供硬件支持。

##### 1.关闭中断方法

单处理器架构--利用“关闭中断disable interrupts”途径：关闭中断后，当前执行的代码段（临界区）不会被抢占without preemption；“关闭中断”法在多处理结构中太低效，不能解决问题。

除此之外，增加硬件指令也会导致CPU结构变得更加复杂，是一个需要很慎重的事情。特别是在多CPU中，关中断难道要关闭所有CPU的中断信号吗？所有这种方法是不行的，还需要找其他方法。

##### 设置原子性指令方式

现代机器提供了特殊的原子性的指令。atomic=不可中断。一种是测试并赋值test memory word and set values。一种是交换内存内容swap contents of two memory words.

##### 2.Test and set原子指令

测试并赋值，定义：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229211610154.png" alt="image-20201229211610154" style="zoom:80%;" />

不是说硬件指令吗，为什么是一个函数？其实在计算机组成中我们学习了微指令，CPU的一些复杂的硬件指令也都是通过组合微指令而成的，写成函数的形式其实很自然，这里只不过是用C语言的格式描述了而已。**由于这是一个硬件指令，所以在它执行完之前，是不可能有中断发生的，这一点就避免了许多许多问题**

<img src="https://www.z4a.net/images/2020/02/06/image-20200205204050844.png" alt="image-20200205204050844.png" style="zoom: 50%;" />

有了硬件指令后，就可以很简单地加一个锁就完事了。这种方式没有对进程的数量进行限制，是无限的。但是这个方法不是没有问题的，它不满足bounded waiting:例如，一个进程设置了锁为true，但是时间片到期，CPU调度给了其他进程，但是由于已经上锁，其他进程无法访问共享资源，只有等待原进程被重新调度。但是如果原进程被调度后执行完了一次后又循环，又加上了锁，这就可能导致其他进程永远不能被执行，违背了bounded waiting的有限次数要求

##### 3.Swap contents of two memory words

<img src="https://www.z4a.net/images/2020/02/06/image-20200205205818990.png" alt="image-20200205205818990.png" style="zoom:50%;" />

<img src="https://www.z4a.net/images/2020/02/06/image-20200205205827490.png" alt="image-20200205205827490.png" style="zoom:50%;" />

这个指令的效果和用TestAndSet没有区别，所以它也不符合bounded waiting。

另外，上面的办法都有一个问题没有解决，就是**对应用程序编写者要求太高**，上面的代码都是要加在应用程序开发代码中的，这对于应用程序开发者是一个很大的负担。而且，一个程序中可能有多个共享资源，也就有了多个临界区，要求开发者对每一个地方都插入上述的代码并进行调试显然是强人所难。这就是下面的解决方案要解决的问题。

## 信号量Semaphores

##### 1.基本概念

信号量试图将用户要做的额外步骤都放一起，并保证其是原子操作。

信号量S是个整型变量，S只允许两个标准操作wait()和signal()，又称为P操作和V操作。wait()和signal()是原子操作。atomic operations。

##### 2.如何实现这种信号量？

为信号量设计一个等待队列queue，等待队列的结点node类型一致，都包含2项数据单元：①int value;②pointer to next node in the queue.

等待队列带两个操作函数：

> Block(); //阻塞自己，主动交出CPU，引起新一轮的CPU调度
>
> Wakeup(); //唤醒等待队列里的一个进程，将它移入就绪队列

<img src="https://www.z4a.net/images/2020/02/06/image-20200205212239477.png" alt="image-20200205212239477.png" style="zoom:50%;" />

如果进程的value小于0，就将自己挂到信号量里面的等待队列中并重新进行调度。

<img src="https://www.z4a.net/images/2020/02/06/image-20200205212246275.png" alt="image-20200205212246275.png" style="zoom:50%;" />

如果value<=0，就从等待队列中弹出一个进程并唤醒这个进程，将其移至就绪队列

##### 3.信号量解决无限多进程的临界区问题

<img src="https://www.z4a.net/images/2020/02/06/image-20200205215247266.png" alt="image-20200205215247266.png" style="zoom:50%;" />

这种方法对用户的要求是比较低的，如果有同步问题的话，只要定义一个信号量然后在上下分别加上wait和signal就好了。

共享变量value初始值为1：对于第一个进程，它把value从1减到0，正常执行；第二个、第三个等等之后的进程因为value值运算后都小于0，所以被wait挂起到等待队列。value值如果小于0的话，它的绝对值就是等待的进程个数。等到第一个进程执行完后，value+1,此时value依然是小于0的，就从等待队列拿一个进程唤醒，它访问完共享资源之后继续调用signal,使得value加一，直到value大于0，也就是恢复到1，开始继续按照上述过程执行。

<img src="https://www.z4a.net/images/2020/02/06/image-20200205215257122.png" alt="image-20200205215257122.png" style="zoom:50%;" />

bounded waiting条件能满足的条件是signal里从等待队列中选进程时使用先来先服务或者轮转法，不能按优先级排列，否则如果一直有高优先级的进程进来的话，低优先级的进程也会出现永远得不到服务的情况。

那么，又回到了原本的问题：如何保证wait和signal操作是原子的?最简单的，可以使用关中断的方式，但是这种方法还是一样，代价太高。现在Linux实现了不需要关中断就能保证原子性的算法。不仅如此，它还给应用程序开发人员提供了信号量组的操作，可以一次性对多个信号量进行操作。

##### 4.信号量解决一般性进程同步问题

<img src="https://www.z4a.net/images/2020/02/06/image-20200205215312859.png" alt="image-20200205215312859.png" style="zoom:50%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229214049429.png" alt="image-20201229214049429" style="zoom:80%;" />

信号量S初始值为0，S1, S2分别为进程P1,P2中的语句，执行完S1后，执行signal(S) S的值变为1，大于0，然后执行wait(S), S--后变成0，不会引起阻塞，所以此时继续执行S2。若不先执行P1，则初始S=0，执行了wait(S)，S=-1,小于0，需要将当前进程挂起到等待队列中并重新进行CPU调度。所以这个方式限制了P2只能在P1执行了之后执行。

## 经典同步问题Classic Problems of Synchronization

都可以将信号量作为基本方法进行解决。

##### 1.生产者-消费者问题producer and Consumer

也叫做bounded buffer问题

<img src="https://www.z4a.net/images/2020/02/06/image-20200205230308306.png" alt="image-20200205230308306.png" style="zoom:50%;" />

①假设有一个拥有N个item的缓冲区，定义3个共享数据shared data为：

> int mutex; //信号量mutex, 初始值为1
>
> int full; //定义信号量full,  初始值为0
>
> int empty; //定义信号量，初始值为N

full表示当前缓冲区被占用的格子数量，empty表示当前缓冲区空闲的格子数量。

②对于生产者进程：每次生产empty减一，如果减到0了就阻塞了，生产完full+1

<img src="https://gitee.com/jiadingMayun/Pic/raw/master/img/image-20200205230404075.png" alt="image-20200205230404075.png" style="zoom:50%;" />

③对于消费者进程：每次消费full减一，等减到0就阻塞，消费完empty+1，mutex是为了控制生产者进程和消费者进程不能同时运行的。

<img src="https://www.z4a.net/images/2020/02/06/image-20200205230415198.png" alt="image-20200205230415198.png" style="zoom: 50%;" />

④信号量的使用次序也是很讲究的，如果使用不当就可能会造成死锁：consumer通过wait(mutex),但发现full是0，此时会被挂起，但是还没有执行signal(mutex),所以producer仍无法执行，程序陷入死锁。

<img src="https://www.z4a.net/images/2020/02/06/image-20200205231714173.png" alt="image-20200205231714173.png" style="zoom:50%;" />

⑤死锁和饥饿：饥饿是理论上不是死锁，但是实际上由于其他情况却发生了的进程无法被调度的情况。

<img src="https://www.z4a.net/images/2020/02/06/image-20200205231618378.png" alt="image-20200205231618378.png" style="zoom: 50%;" />

##### 2.读者-写者问题readers and writer

<img src="https://gitee.com/jiadingMayun/Pic/raw/master/img/image-20200205232254942.png" alt="image-20200205232254942.png" style="zoom:50%;" />

①读进程之间不需要互斥，但是写进程和读写都互斥。

> 读锁read lock, or 共享锁share lock
>
> 写锁write lock, or 互斥锁 exclusive lock

②问题是，读写进程同时排队时，先调度读还是先调度写?第一类是读者优先，除非一个写者已经获准访问共享对象集，否则不会让reader等待，但是写者可能会饥饿；第二类是写者优先，读者可能会饥饿。这里以读者优先为例来解决这个问题。

③定义共享数据集：

> 信号量mutex--初始值为1--在读者之间协调
>
> 信号量wrt--初始值为1--解决所有的读者和不同的写者之间的互斥
>
> 整型变量readcount，初始值为0--为读的进程计数

<img src="https://www.z4a.net/images/2020/02/06/image-20200205233013130.png" alt="image-20200205233013130.png" style="zoom:50%;" />

<img src="https://www.z4a.net/images/2020/02/06/image-20200205233021569.png" alt="image-20200205233021569.png" style="zoom: 67%;" />

mutex是为了给readcount加锁，使得readcount保持正确。if(readcount==1) wait(wrt)是检测现在是否有写者，如果有的话就阻塞自己，如果没有的话，wait(wrt)也能避免之后读者在读数据的时候有写者想进入。而正因为有了这样的操作，所以只要读者不是第一个，也就是readcount==1的话，就不需要管写者了，所以才会有这个if语句

之后的if(readcount==0)signal(wrt)是解开写者的锁，这只有最后一个读者才会去解

##### 3.哲学家就餐问题dining -philosophers

<img src="https://www.z4a.net/images/2020/02/06/image-20200205233037847.png" alt="image-20200205233037847.png" style="zoom:50%;" />

哲学家只有两种状态：思考,吃饭；仅五根筷子--**若干进程之间分配若干资源问题**

<img src="https://www.z4a.net/images/2020/02/06/image-20200205233045175.png" alt="image-20200205233045175.png" style="zoom:50%;" />

死锁是显然的：因为每个进程需要两个资源才能正常运行，很容易死锁。可能的解决方法：①留一个空闲资源，不要一次性所有人上座：加一个信号量，初始化为4；②一次性获得资源，不要先后拿取两根筷子，要么拿两根要不不拿；③不对称解：五个哲学家中一部分先拿左手边，一部分先拿右手。这种可以解决死锁（因为只要这些人中有不一致的，就一定有至少一个人能从左右手边拿到一对筷子），但是不能解决饥饿。

## 管程Monitors

##### 1.概念

信号量提供了方便的机制处理进程同步，但不正确的使用信号量仍会导致时序错误，且难以检测。如：

- 先对信号量 `signal()` 再 `wait()` 违反了互斥请求
- 对信号量始终调用 `wait()` 将导致死锁
- 一个进程遗漏了 `wait()` 或 `signal()` 将导致死锁且可能破坏互斥

管程（monitor）提供了一组由程序员定义的、在管程内互斥的操作。管程内定义的子程序只能访问位于管程内的局部变量和形式参数，管程内的局部变量也只能被管程内部的局部子程序访问。管程结构确保了同时只能有一个进程在管程内活动。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229222457118.png" alt="image-20201229222457118" style="zoom: 80%;" />

语义视图：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229222527754.png" alt="image-20201229222527754" style="zoom: 80%;" />

##### 2.条件变量condition variable

管程内部可定义 `condition` 类型的变量以提供同步机制，称其为条件变量。条件变量可执行操作 `wait()` 和 `signal()`。

条件变量存在于管程内部，对同一个条件变量调用操作的进程将和条件变量建立一定的联系，或者称之为绑定。对于管程内的条件变量 x，进程 P 调用 `x.wait()` 将时自身挂起到条件变量 x 上；当另一个进程调用 `x.signal()`时，在 x 上悬挂的进程会被重启，如果此时没有进程悬挂在 x 上，则 `x.signal()` 操作将被忽略。

管程模式下的 `x.signal()` 和信号量的 `signal()` 区别在于： **信号量操作 `signal()` 会影响信号量的状态** ，而管程下的 `x.signal()` 在 x 不存在挂起进程的情况下没有任何影响。

有条件变量的管程模型：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229222912308.png" alt="image-20201229222912308" style="zoom:80%;" />

举例：进程 P 调用 `x.signal()`，且存在悬挂进程 Q 与条件变量 x 关联。根据管程的性质，若进程 Q 开始执行，则进程 P 必须等待。此时可能存在两种可能性，且两种可能性均有合理解释：

- 进程 Q 重启且进程 P 等待：进程 P 将等待，直到进程 Q 离开管程或者等待另一个进程调用 `x.signal()`
- 进程 P 唤醒进程 Q 且进程 P 继续执行：进程 Q 被唤醒，但仍然会等待，直到进程 P 离开管程，或者另一个触发条件。因为 P 已经在管程中执行，看起来此种方案更合理，但这破坏了进程 Q 正在等待的逻辑条件，进程 Q 已被触发但又未执行，因此状态难以描述

Pascal 语言采用折中方式，当进程 P 执行 `x.signal()` 时，它会立刻离开管程，且进程 Q 会立刻重新执行。

##### 3.哲学家就餐问题的管程解法

使用 **[进程同步](http://blog.forec.cn/2016/11/24/os-concepts-5/)** 中的一种策略：当哲学家在两只筷子均可用的情况下才拿起筷子，且拿起两只筷子的动作是非抢占的。

为哲学家设置三种状态：`enum {THINKING, HUNGRY, EATING} state[5]`

哲学家 i 只有在两个邻居都不进餐时才能将变量 `state[i]` 设置为 `EATING`，当他处在饥饿状态又无法进餐时可以使自己忍耐一段时间：`(state[(i-1)%5] != EATING) && (state[(i+1)%5] != EATING)`

下面给出用管程解决的哲学家进餐问题，只解决了互斥问题，不会导致死锁，但可能导致某个哲学家过度饥饿而死。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229223439110.png" alt="image-20201229223439110" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229223454176.png" alt="image-20201229223454176" style="zoom: 80%;" />

##### 4.使用信号量实现管程

要实现的管程对于重启进程采用的策略是： **调用 `x.signal()` 的进程挂起自己，直到重新启动的进程离开或者等待** 。

每个管程都有一个信号量 `mutex` 初始化为 1，进程进入管程之前必须通过 `wait()` 获得允许，离开时需要调用 `signal()` 释放权限。

信号量 `next` 初始化为 0，供线程在唤醒重启进程时挂起自己，整数变量 `next_count` 用于对挂起在 `next` 上的进程数量计数。

进入管程的外部子程序结构 F 如下：

```c
void F(){
    wait(mutex);
    // 子程序执行
    // ...
    // 子程序执行结束
    if (next_count > 0)
        signal(next);    //     此前有进程挂起，重启该进程
    else
        signal(mutex);   //     管程内无进程挂起，释放控制权
}
```

对每个管程内的条件变量 `x`，引入信号量 `x_sem` 和整数变量 `x_count` 记录信号量 x 上挂起的进程数量，均初始化为 0。`x.wait()` 和 `x.signal()` 实现如下：

```c
void x.wait(){
    x_count++;            // 将进程挂起到 x 上
    if (next_count > 0)   // 当前仍有进程挂起在管程中
        signal(next);
    else
        signal(mutex);    // 无进程在等待，释放管程控制权
    wait(x_sem);          // 等待信号量 x_sem，由信号量决定唤醒哪个挂起进程
    x_count--;            // 等待结束，进程被唤醒
}

void x.signal(){
    if (x_count > 0){     // 当前有程序挂起在条件变量 x
        next_count ++;    // 自己将要被阻塞，故管程挂起数增加
        signal(x_sem);    // 释放信号量，唤醒一个挂起进程
        wait(next);       // 将自身阻塞到管程中
        next_count--;     // 被唤醒，继续执行
    }
    // 没有程序挂起在条件变量 x，不产生任何影响
}
```

管程和信号量的对比：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229223923938.png" alt="image-20201229223923938" style="zoom:80%;" />

## Synchronization Examples

##### 1.Solaris

##### 2.Windows XP

##### 3.Linux

##### 4.Pthreads

## 原子性事务Atomic Transaction

##### 1.系统模型system model

确保操作作为一个单一的逻辑工作单元发生，完整的，或根本没有。与数据库系统领域有关。挑战是确保原子性，尽管计算机发生系统故障，也要保证位操作的roll back。

事务transaction-执行单个逻辑函数的指令或操作的集合。在这里，我们关注的是稳定存储-磁盘的变化。假设事务是一系列读写操作。以commit成功结束或以abort失败告终。aborted中止的事务必须回滚roll back以撤消undo其执行的任何更改。

存储介质storage media的类型：①volatile易失性存储，如主存，cache；②非易失性存储nonvolatile storage，如磁盘和磁带。③稳定存储stable storage，不会丢失信息，实际上是不可能存在的，通过复制或RAID模型逼近具有独立故障模式的设备。

我们的目标是当故障导致丢失有关易失性存储的信息时，确保事务的原子性。

##### 2.基于日志的恢复机制log-based recovery

日志记录事务所有修改的稳定存储信息。

最常见的是write-ahead logging：将日志记录在稳定存储介质上，每条日志记录描述单个事务写入操作，包括①事务名；②数据项名；③old value；④new value。

> <Ti start>当事务Ti开始，将该指令写入日志。
>
> <Ti commit>当日志结束，提交时，写入日志。

在对数据进行操作之前，日志条目必须达到稳定的存储reach stable storage。日志应该按照操作顺序写入，需要先写日志，然后更新数据库。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229225702947.png" alt="image-20201229225702947" style="zoom:80%;" />

使用日志，系统可以处理任何易失性内存错误：

> Undo(Ti) ; //恢复Ti更新的所有数据的旧值
>
> ● Redo(Ti); //将事务Ti中所有数据的值设置为新值

Undo(Ti)和Redo(Ti)必须是幂等idempotent的--多个执行必须具有与一个执行相同的结果。如果系统失败，通过日志恢复所有更新数据的状态：

> If log contains <Ti starts> without <Ti commits>, undo(Ti) 
>
> If log contains <Ti starts> and <Ti commits>, redo(Ti)

##### 3.检查点checkpoint

日志可能会很长，恢复可能需要很长时间。检查点缩短日志和恢复时间。

检查点方案：

- 将当前在易失性存储中的所有日志记录输出到稳定存储中
- 将所有修改后的数据从易失性存储输出到稳定存储
- 将日志记录<Checkpoint>输出到稳定存储的日志中

那么现在恢复只包括Ti，这样Ti在最近的检查点之前开始执行，而在Ti之后所有事务都开始执行。 所有其他事务已经稳定存储：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229230208114.png" alt="image-20201229230208114" style="zoom:80%;" />

##### 4.并行原子事务concurrent atomic transactions

必须等同于串行执行-可串行性serializability，可以在临界区critical section执行所有事务。并发控制算法提供了可串行化。

串行化

非串行化调度

并行可串行化调度

锁协议

两阶段锁协议

基于时间间隔的协议

##### 5.ACID

原子性atomicity+一致性consistency+独立性isolation+持久性durability