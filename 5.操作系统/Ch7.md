# Ch7 DeadLock

## 死锁问题

##### 1.定义

一组被阻塞blocked的进程，每个进程都持有一个资源，并等待该集合中另一个进程持有的资源。A set of blocked processes each holding a resource and waiting to acquire a resource held by another process in the set.

例子：系统中有两个进程P1,P2，他们两个都等待对方进程释放资源从而开始执行。形成请求的循环。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229161501316.png" alt="image-20201229161501316" style="zoom:67%;" />

例：车队过独木桥

<img src="https://gitee.com/jiadingMayun/Pic/raw/master/img/image-20200206103541544.png" alt="image-20200206103541544.png" style="zoom: 50%;" />

## 关于死锁的系统模型

<img src="https://gitee.com/jiadingMayun/Pic/raw/master/img/image-20200206103858513.png" alt="image-20200206103858513.png" style="zoom: 50%;" />

<img src="https://gitee.com/jiadingMayun/Pic/raw/master/img/image-20200206104055466.png" alt="image-20200206104055466.png" style="zoom:50%;" />

进程使用资源的过程：request--> use-->release

## 死锁状态的四大特征

##### 1.基本特征

如果以下四个条件同时存在，就会出现死锁：

- 互斥性Mutual exclusion：一次只有一个进程可以使用资源。表示该资源必须是互斥访问的，这是设备的固有特性导致的，例如打印机、信号量
- hold and wait：持有至少一个资源的进程正在等待获取其他进程持有的额外资源。
- no preemption：资源只能在该进程完成任务后由持有它的进程自愿释放
- circular wait：存在一组等待资源的进程{P0，P1，...，P0}，使得P0等待由P1持有的资源，P1等待由P2持有的资源，...，Pn-1等待由Pn持有的资源，Pn等待由P0持有的资源

注意这些是死锁产生的必要条件，也就是说即使满足这些条件，也不一定产生死锁

##### 2.资源分配图resource-allocation graph

一些列的顶点V和边E，其中V被分成两种类型：①P类型={P1,P2,...Pn}代指系统中所有的进程；②R={R1,R2...Rn}呆滞系统中所有的资源类型。

request边--用有向边Pi->Rj表示进程Pi请求资源Rj

assignment边--用有向边Rj-->Pi表示资源j被分配给了进程i.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229162505426.png" alt="image-20201229162505426" style="zoom:80%;" />

资源分配图：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229162541395.png" alt="image-20201229162541395" style="zoom:80%;" />

有死锁的资源分配图：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229162612719.png" alt="image-20201229162612719" style="zoom:80%;" />

有一个环但是无死锁的资源分配图：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229162656802.png" alt="image-20201229162656802" style="zoom:80%;" />

如果图不包含回路⇒则一定不包含死锁。如果图包含一个回路：如果每个资源类型只有一个实例，则存在死锁；如果每个资源类型有几个实例，可能存在死锁。

## 处理死锁问题的思路

对于死锁，操作系统的处理态度可以是不一样的：操作系统可以选择预防死锁产生，也可以允许死锁产生但是在死锁产生后进行干预，也可以对死锁完全不理睬。

主要是思路有：①确保系统永远不会进入死锁状态。②允许系统进入死锁状态，然后恢复。③忽略这个问题，假装系统中从未发生死锁；大多数操作系统，包括UNIX 使用这种思路。

## 死锁预防prevention

死锁预防的主要思路就是保证任何情况下，四个条件不能同时满足。

##### 1.mutual exclusion

可共享的资源自然不满足此条件，不可以共享的资源必然满足此条件。第一个条件由于是设备的固有特性，不好处理.

##### 2.hold and wait

确保一个进程申请一个资源时，它没有占用其他资源：①策略1--进程开始执行前，它已经申请并获得所有的资源了；②只允许在不占有资源的情况下申请其他资源。

要么不允许动态申请资源，要么只能在不占用资源的情况下动态申请资源，显然限制太大。因为进程不是一直使用资源的，但是第一种方法却使得进程一直占用资源；而第二种对进程的功能限制太大，也可能导致需要资源比较多的进程导致饥饿。

##### 3.no preemption隐含抢占

一个进程已经拥有了资源，假如它试图申请其他资源，但是没有马上得到满足，它不得不等待；同时要求它释放已经拥有的所有资源。

以这种途径释放的资源，用来重新分配给等待队列里的进程。当且仅当一个进程既能重获它进入等待队列时释放的资源，又能分到它当前申请的资源，该进程才被重新分配CPU执行。

这个思路看起来比前面的靠谱一些，但是还是存在一些问题，就是这个过程对于进程必须是无损的，但是**不是所有的资源都能实现隐含抢占对进程的无损**，一些不可共享的资源，例如信号量，就不能这样做。

##### 4.circular wait

为所有类型的资源定义一个顺序号；一个进程先后申请的资源，其资源类型的顺序号必须单调递增。

意思就是，如果已经申请了8号资源，那么所有编号小于8的资源都不可以再申请了。显然，越常用的资源应该编号越小。这种方法能避免循环等待的“循环”条件：占用了大编号的资源的进程不可以再申请小编号的资源，所以避免了环的形成。

但是这种方法有两个突出的问题：第一个就是这个顺序号很难确定，因为许多资源就是普通的文件，很难说它的级别是多少。另外，这种方式导致了进程如果想要先获取大编号的资源，再获取小编号的资源，这种方式是不被允许的，只能在申请大编号资源之前先把小编号资源获得了，这降低了资源利用率。代价很大，或者不太可行。

**综上，死锁的预防是很难实现的，所以目前的实际应用不是预防**。

## 死锁避免avoidance

##### 1.基本概念

前提是系统拥有先验知识a prior information，知道每个进程将如何利用资源。简单而直观的要求，则是每个进程事先申报每种类型资源的最大需求数。

死锁避免算法动态的检测资源分配状态resource-allocation state，它总是确保circular-wait条件永远不成立。资源分配状态取决与三个元素：available可分配资源数、allocated已分配资源数、maximum demands进程对每种类型资源的最大需求数。

##### 2.安全状态safety state

安全的资源分配状态。当一个进程申请可用的资源时，系统必须判断：此次分配后，系统是否仍然处于安全状态。

如果存在一个序列<P1, P2, P3,...Pn>，对，进程Pi，j<i其中进程Pi将要申请的资源总量<(当前可用的资源+所有其他进程Pj占有的资源)，那么该系统处于安全状态。

对定义的解读：如果进程Pi需要的资源不能立即到位，它可以等待有限的时间，直到进程Pj释放相关资源；进程Pj运行结束，进程Pi即从进程Pj获得它需要的资源，然后执行，然后释放所有资源。进程Pi结束后，进程Pi+1从进程Pi获得它需要的资源，以此类推。

实例：

<img src="https://i.loli.net/2020/02/06/DFrzUnLZtAH48oE.png" alt="image-20200206112043479.png" style="zoom:50%;" />

存在安全序列<P1, P0, P2>，所以该系统是安全状态。

这个例子很好的说明了安全状态的概念：安全状态下，我们可以通过先满足排在序列前面的进程的要求，从而在其执行完后释放其的资源，以满足排在后面的进程的要求，最终满足所有进程的要求。也就是说，**满足要求的顺序就是安全序列的顺序**

如果系统总是处于安全状态，则不会存在死锁；如果系统某时刻不在安全状态，可能存在死锁，也可能不存在死锁。可以通过确保系统不进入“不安全状态”实现死锁的避免。



<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229164816630.png" alt="image-20201229164816630" style="zoom:80%;" />

检测算法--①若一种资源类型只有 一个实例，使用资源分配图来进行检测；②如果一种资源类型有多个实例，使用银行家算法banker's algorithm

##### 3.资源分配图机制

Claim Edge: Pi→Rj表示进程Pi可能请求资源Rj；用虚线表示。Claim Edge转换成request edge当该进程提出请求资源的请求request。request edge转换成assignment edge当该资源被分配给给进程。assignment edge转换成claim edge当该进程请求的资源被释放了。

资源必须先验地在系统中认领。资源分配图如下：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229184026355.png" alt="image-20201229184026355" style="zoom:80%;" />

资源分配图的不安全状态：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229184111892.png" alt="image-20201229184111892" style="zoom:80%;" />

如果P1请求request资源R2，就会产生死锁。这里默认一种资源只有一个实例。

假设进程PI请求资源Rj，只有当请求边request edge转换为assignment edge不会导致资源分配图中的回路cycle形成时，才能授予请求request can be granted.

##### 4.银行家算法

前提：①一种资源可以有多个实例；②每个进程都要知道它的最大资源使用量maximum use；③当进程request一个资源时，它可能要等待；④当一个进程获得所有资源时，它必须在有限的时间内运行完并返回释放它们。

基本数据结构： 

- n--进程数；m--资源类型数；
- available[m]--长度为m的数组，available[j]=k表示Rj类型的资源共有k个实例可用
- max，n*m矩阵，max-ij=k表示进程Pi可以申请最多k个Rj类的资源，max就是最大需求矩阵,即完成该进程总共需要这么资源。
- allocation--n*m矩阵，allocation-ij=k，表示进程Pi拥有Rj类型的资源k个
- Need-n*m矩阵，need-ij=k表示进程Pi还需要申请Rj类资源k个才能完成任务
- need-ij=max-ij - allocation-ij

算法基本内容：

- 令work为长度为m的矢量，finish为长度为n的矢量，初始值work=available, finish[i]=false,for i=0....n-1
- 选取满足如下条件的i：finish[i]=false, needi<=work（即未完成运行的，且当前的资源可以让它运行的），如果不存在，跳到第四步
- work=work+allocation-i; finish[i]=true; go to step 2.就是代表了执行完这个可以运行的进程之后，释放该进程的所有资源，并将该进程的状态标记为已完成。
- 如果对所有i，都有finish[i]=true,即所有进程都能顺利执行完，则该系统安全。不存在死锁，否则就说明系统不安全。

银行家算法响应进程Pi的请求的过程：

- 定义request数组，request-i[j]=k表示进程Pi发出请求，申请Rj类型的资源k个。

- 如果request<=Needi,说明请求的资源数小于该进程需要的，继续算法；否则报错，因为进程申请数超过了预报数。

- 如果request<=available，继续算法；否则进程Pi等待，因为系统现有的资源available数小于进程请求的，不够用只能等待；

- 如果，如进程所愿，它得到了申请的资源，则进行以下计算：

  > available=available-request；//系统当前有的该类资源减少这么多
  >
  > allocation-i=allocationi+request；//进程该类资源增加
  >
  > need-i=need-i - request-i;	//进程的需求数减少这么多

- 调用safety算法，如果返回safe则不会死锁，资源如数分配给进程Pi,如果返回unsafe，进程Pi等待，数据结构available，allocation，need恢复到调用整个该request算法前的值。

<img src="https://www.z4a.net/images/2020/02/06/image-20200206113553503.png" alt="image-20200206113553503.png" style="zoom:80%;" />

如果安全，就真的分配给该进程，否则就将该进程挂起，不让它加入安全队列中。

##### 5.示例

银行家算法实例：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229191336398.png" alt="image-20201229191336398" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229191354598.png" alt="image-20201229191354598" style="zoom:80%;" />

首先用max-allocation计算need数组，即各个进程此时还需要的资源数。available数组是当前系统中所有空闲的资源，是总的资源减去当前各个进程的已经占有的资源总数。



<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229191426088.png" alt="image-20201229191426088" style="zoom:80%;" />

**也就是说，一旦有进程提出申请，就更新状态，然后判断是否依然是安全状态**，效率不太高。

## 死锁检测detection

上面为了避免死锁的产生，所用的方法都是代价很大的，而这里的思路就是从避免死锁产生到死锁产生后如何即使发现和修复。

容忍系统陷入死锁状态；设计死锁检测算法，供需要时调用。配套系统恢复的对策。

##### 1.每种资源只有一个实例的情况

维系着一个wait-for图：结点是一个一个的进程；Pi->Pj表示Pi在等待Pj。定期调用在图中检测是否有回路的算法。如果存在回路，则存在死锁。

一种在图中检测回路的算法需要n^2的复杂度，其中n是图中的顶点数。

资源分配图和等待图：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229192831111.png" alt="image-20201229192831111" style="zoom:80%;" />

深度优先搜索算法......

##### 2.每种资源可以有多个实例的情况

基本的数据结构：

- available--长度为m的数组，available[i]=k，表示资源Ri有k个空闲的实例
- allocation-ij=k，表示进程Pi当前占有资源Rj一共k个
- request-ij=k,表示进程Pi当前请求了Rj类的实例共k个。

检测算法：

- 令work为长度为m的矢量，finish为长度为n的矢量，初始值work=available; for i=1,2,3...n，如果allocation[i]≠0，则finish[i]=false，否则为true,即将当前占有资源的进程记为false，否则为true.
- 选取满足条件finish[i]==false；request[i]<=work的i,继续算法；若不存在这样的i，则跳到第四步。即找一个i满足，该进程当前占有一定资源且请求的资源小于系统当前空闲的资源数。
- work=work+allocation;//该进程获得资源，执行完，然后释放之前已经占有的资源；finish[i]=true;//当前该进程已经运行完毕，不再占有任何资源，跳转到步骤2
- 如果所有的finish[i]都为true则所有进程最周占有的资源都为0，所有进程都能顺利运行完毕。如果存在i使得finish[i]==false，则系统处于死锁状态，且进程Pi死锁了。

**死锁检测算法只需要在怀疑系统出现死锁的时候调用即可**

死锁检测算法实例：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229195149477.png" alt="image-20201229195149477" style="zoom:80%;" />



<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201229195137468.png" alt="image-20201229195137468" style="zoom:80%;" />



可以看到，死锁检测算法其实就是死锁避免算法。那么，应该在什么时候调用该检测算法呢？可以有两种思路：

- 定时运行：和系统一般产生死锁的时间间隔向配套，如果经常生产死锁，运行间隔的周期就短一点，反之就长一点，这样，**死锁对进程的最长影响时间就是一个运行间隔**
- 监测运行：如果CPU利用率低于阈值就运行一下（因为CPU利用率低可能就是死锁造成的，另外CPU利用率低的时候运行对效率影响比较小）。或者是如果有进程请求很大量的进程时运行一下

当然，这两种思路不是互斥的，两条规则可以都应用上。

## 死锁恢复recovery

有两种思路：①所有死锁进程全部挂掉；②每次只杀死一共进程，直到系统脱离死锁状态。

第二种方法相对温柔一些（但是第一种方法也有实际的操作系统在用），那么如何决定先杀死哪个进程呢？有多种判断依据：

<img src="https://www.z4a.net/images/2020/02/06/image-20200206145641036.png" alt="image-20200206145641036.png" style="zoom: 50%;" />

实际使用的时候不能使用单一判决，可以使用多种判决计算综合代价，它的算法折射了哪些进程在该操作系统看来更加重要。

<img src="https://www.z4a.net/images/2020/02/06/image-20200206150030850.png" alt="image-20200206150030850.png" style="zoom:50%;" />

如果保存了进程的image,可以做的不将进程彻底杀掉，而是回退到某个时间点。