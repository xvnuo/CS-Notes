# Ch9 Virtual Memory

## 一 背景

##### 1.虚拟存储概念

逻辑空间可以独立于物理空间。

观察进程对于物理空间的需求：①进程只需要一小部分的代码(请求CPU执行的代码部分)驻留内存；②进程的逻辑空间可以远大于(分配给它的)物理空间。于是，物理空间被更多的进程共享。**进程的逻辑空间可以远大于分配给它的物理空间，因为每次只装入一部分代码**

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227215705945.png" alt="image-20201227215705945" style="zoom:80%;" />

虚拟存储可以通过①demand paging和②demand segmentation实现。

进程虚拟空间的典型映像image：如果不动态申请的话，heap可以放在外面

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227215758638.png" alt="image-20201227215758638" style="zoom:80%;" />

虚拟存储依然可以支持共享空间。系统库System library可以通过将共享对象映射到虚拟地址空间实现在多个进程之间共享。进程创建过程中可以共享页pages，以便加速创建过程。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227220214584.png" alt="image-20201227220214584" style="zoom:80%;" />

## 二 按需分页demand paging

##### 1.按需调页demand paging

当且仅当一个page被需要，才将他放入内存，这样可以获得：更少的I/O, 更少的内存，更快的响应，更多用户。

Lazy swapper--从不换入这个页面，除非有进程真的访问这个页面了。处理页的swapper被称为pager。

应用程序代码只有一部分是在执行时候需要的，其他的都是异常处理或者不常用的模块，这些代码没必要装载进来但是它使用中断，还有io操作，效率上也有损失。但是正因为它的好处非常明显，现代操作系统基本上全都使用了虚拟存储技术

CPU指令含内存访问，即访问该内存地址所在页面。称作页面引用(reference)
存在3种可能，①页面已经装入内存，有对应帧=>CPU完成对应操作。②非法的页面引用 => abort；③合法引用，但是页面不在内存=> 把该页装入内存.

如何判断页面是否已经装入内存？**已经装入内存的页page可以在页表上查询到页-帧项**

**物理内存被装满怎么办？这就需要把其他进程的占用部分换出，放置到外存中**，再将该进程的代码换入。页换入换出示意图：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227220822198.png" alt="image-20201227220822198" style="zoom:80%;" />

##### 2.有效位valid-invalid bit

每个进程的每个页表项，都有一个“有效位”，a valid-invalid bit。有效位为v,表示该页在内存中；有效位=i,表示进程非法访问该页，或者，该页不在内存。如图：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227221313839.png" alt="image-20201227221313839" style="zoom:80%;" />

初始时，所有“有效位”都设置成i。CPU执行指令，访问内存单元时，一旦读到某个页表项，其“有效位”是i，就会产生缺页page fault。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227221335239.png" alt="image-20201227221335239" style="zoom:80%;" />

##### 3.缺页page-fault

第一次引用某页面，对应页表项的“有效位”为i，引起缺页中断。操作系统响应这个中断的过程: ①操作系统查找内核的数据结构，判断: 若它是非法引用=> abort；若它是合法引用，但是页面不在内存中，则往下；②操作系统查找内核的数据结构，找出一个空闲页帧，③把页面从外存换入至该空闲页帧，④更新内核的数据结构，更新进程的页表；⑤把进程页表中(该页面)页表项的“有效位”重置为v;⑥缺页中断程序返回；⑦重新执行引起缺页中断的那条指令。缺页中断及其响应过程：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227221732913.png" alt="image-20201227221732913" style="zoom:80%;" />



##### 4.性能分析

设缺页率page fault rate: 0<=p<=1.0;

有效访问时间effective access time, ECT=(1-p)*内存访问时间+p×(缺页中断处理开销+页面换出时间+页面换入时间+重新执行指令的开销)

memory access, page fault overhead, swap page out, swap page in, restart overhead 

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227222759697.png" alt="image-20201227222759697" style="zoom:50%;" />

**可见缺页率对于效率有多大的影响**，所以必须要有一个非常合适的置换算法。

##### 5.进程创建

虚拟存储在创建进程时还有其他好处：①copy-on-write;②memory-mapped files

## 三 Copy on Write

Copy-on-Write算法意思就是：父进程和子进程在最开始共享内存中的同一页same pages,若任意一方修改了modified共享页，这时就要复制该页，以使得该修改不会影响到另一方。

该机制更加高效，不用每次fork()都复制页。

进程1修改共享页C之前

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227223745956.png" alt="image-20201227223745956" style="zoom:80%;" />

进程1修改C之后：进程1重新指向修改后的C的副本

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227223819233.png" alt="image-20201227223819233" style="zoom:80%;" />

## 四 页面置换算法Page Replacement

##### 1.概念

设计利用modify(dirty)bit来减少页面传输的开销。换出页面时就判断：该页面装入后，没被修改过，就不需要换出，可直接丢弃。因为交换区里有他的备份。只有被修改过的页面才有必要换出，页就是更新交换区里的备份。

**注意这个“换出“的意思是和磁盘中的内容同步，是要写的**。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227224517969.png" alt="image-20201227224517969" style="zoom:80%;" />

在内存中找出某个逻辑页面，把他换出，需要考虑：①页面置换算法；②性能的影响--选中的页面置换算法，使之引起的缺页中断次数最少。由于程序执行的不可知性，同一个页面可能会装入很多次。

**这里要注意，页面置换算法只有在物理内存满的时候才会被调用，客观上来说被调用的次数并不多**，但是每次调用对效率的影响都是挺大的

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227223209597.png" alt="image-20201227223209597" style="zoom:50%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227224831292.png" alt="image-20201227224831292" style="zoom:80%;" />

##### 2.置换算法

追求最低的缺页率，评估方法：让算法响应一串特定的内存引用(引用穿，reference string)，累计其缺页次数。

缺页次数相对于物理帧数目的关系：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227225110339.png" alt="image-20201227225110339" style="zoom:80%;" />

##### 3.FIFO页面置换算法

first-in-first-out.先进来的页先被换出。例：3个物理页帧，每个进程用3个页

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227225315485.png" alt="image-20201227225315485" style="zoom:80%;" />

最后的1，2没有发生缺页，新的1，2页覆盖了也不算更新，所以此时还是1最老，2其次，5算最年轻的。

Belady's Anomaly: more frames=》 more page faults，物理页帧多了，缺页反而多了，算法本身带来的异常，所以实际并不适用该算法。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227230044039.png" alt="image-20201227230044039" style="zoom:80%;" />

##### 4.Optimal算法

置换掉最长时间不被引用的页。这其实是最优的算法，也就是说它的缺页次数实际上已经是最少的了。但是这是不实际的：”最长时间“指的是今后的时间，而不是之前的时间，而我们是不知道之后的页使用情况的。所以说这个算法是“可望不可即”

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227230539546.png" alt="image-20201227230539546" style="zoom:80%;" />

##### 5.最近最少被使用算法Least Recently Used Algorithm

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227230724914.png" alt="image-20201227230724914" style="zoom:80%;" />

如何知道多长时间没被引用？

--利用计数器实现：每个页表项带一个计数器，每当引用页面时，页表项的计数器更新为当时的时钟值；调用置换算法时，选取计数器最早的页面，换出即可。时钟值是从开机时间计算，初始值是0。这种算法可以实现了，但是时间开销很大，尤其是如果我们要在所有进程的所有页表项中找的话。

--利用堆栈实现：设计双向链表，维护一个堆栈，当引用某页时，将该页面移动到栈顶；久而久之，链表的尾部就是最长时间最少使用的页面了。页面置换时，不需要搜索、选取页面。但是每次引用时，找该页面的节点比较花时间，花费时间更多了，为了减少时间开销，有以下优化方案：

##### 6.近似LRU算法：LRU Approximation Algorithm

--引用位reference bit：①每个页表项设计1位引用位，初始值=0；②页面被访问时，引用位被置为1；③需要置换页面时，总是选取引用位为0的页面；死近似算法不规定所有引用位为0的页面的顺序。

这种方法的问题也是显然的：①不一定存在这样的页面②目前为止没有被修改过不一定代表是最近最少被访问的页面

--Second chance算法，也叫做clock算法：①也需要引用位；②需要置换页面时，顺时针考察下一页面，如果引用位是0则选中；如果引用位是1，引用位被置为0，留在内存，这次不选他；③考察下一个页面（按照顺时针顺序），重复上述程序判断引用位。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227231750280.png" alt="image-20201227231750280" style="zoom:80%;" />

它的时间开销比较小，是实际可行的。设置一个计数器的话就可以实现其他的算法：

##### 7.计数counting算法

<img src="https://www.z4a.net/images/2020/02/07/image-20200206232449108.png" alt="image-20200206232449108.png" style="zoom: 50%;" />

LRU算法在这几个中相比是最现实可行的，所以现在的操作系统都在使用LRU算法进行页面置换，甚至于像页式内存管理中的TLB中，对于哪些页表项要存在TLB中、满了怎么办，也是使用的LRU算法。

## 五 Allocation of Frames

讨论：进程刚刚创建时如何分配内存？

每个进程需要最少一定数量的页面pages--通常由计算机系统架构决定该数量，才能运行。两种主要的帧分配策略：①fixed allocation；②priority allocation。

##### 1.固定式分配fixed allocation

<img src="https://www.z4a.net/images/2020/02/07/image-20200207000653890.png" alt="image-20200207000653890.png" style="zoom:50%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227232513184.png" alt="image-20201227232513184" style="zoom:67%;" />

##### 2.Priority Allocation

<img src="https://www.z4a.net/images/2020/02/07/image-20200207000749780.png" alt="image-20200207000749780.png" style="zoom: 50%;" />

这里归纳了全局分配global和局部分配local两种思路：

<img src="https://www.z4a.net/images/2020/02/07/image-20200207000858884.png" alt="image-20200207000858884.png" style="zoom: 50%;" />

全局置换的问题--不可预测的缺页率，不能控制他自己的缺页率。

局部置换的问题：吞吐量低，一个进程闲置的帧不能被别的进程使用。

## 五 抖动Thrashing

##### 1.概念

<img src="https://www.z4a.net/images/2020/02/07/image-20200207000958714.png" alt="image-20200207000958714.png" style="zoom:50%;" />

CPU利用率低是因为忙于做换入和换出，都在做io操作，使得从指标上有欺骗性，不搭配IO使用率看不容易发现。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201227233020276.png" alt="image-20201227233020276" style="zoom:80%;" />

增加进程数，CPU利用率反而下降了，这就是发生了抖动。

##### 2.按需调页和抖动demand paging and thrashing

<img src="https://www.z4a.net/images/2020/02/07/image-20200207002452860.png" alt="image-20200207002452860.png" style="zoom:50%;" />

##### 3.计算Locality--workSet

可以使用工作集的方式来计算Locality，Δ ≡ 工作集窗口working-set window ≡ 固定数据的页面引用a fixed number of page references

例如：10000条指令，进程Pi的工作集(working set size of Proecss Pi, WSSi)=最近一次Δ(随时间变更)的页面引用总数。①Δ太小，则无法覆盖完整的locality，②Δ太大，跨越若干localities；③Δ=正无穷，则会覆盖整个程序，因此没有意义。

D = ΣWSSi = 系统所有进程的页帧总需求数；①当D>m，会发生抖动。②应对策略：如果D>m则挂起一个进程。其中，m是物理内存的总的页帧数。

使用工作集方式，用各个进程的工作集的和（它们是没有重叠的，因为不同进程引用的是各自的页面）作为近似的locality（用一段时间的进程引用观测值近似locality的量），如果比总页帧数大，说明物理内存早晚不能满足这些进程的要求，就需要杀掉一些进程。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201228192753712.png" alt="image-20201228192753712" style="zoom:80%;" />

用一个间隔计时器interval timer和一个参考为reference bit来进行近似。如Δ=10000，①计时器每5000个时间单元中断一次，②在内存中为每个页page保存2个bits，③当计时器中断时interrupts，将两个参考位都设为0；④如果有一个bit为1，就说明该page在working set中。这不完全准确？因为无法判断5000个单位的参考发生在哪里。

##### 4.page-fault缺页的频率模式frequency scheme

为每个进程建立一个acceptance缺页率page-fault rate.如果实际的比率太低，进程丢帧，如果实际的比例太高，进程获得帧。由此实现平衡。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201228193717188.png" alt="image-20201228193717188" style="zoom:80%;" />

## 六 Memory-Mapped Files

内存映射文件I/O(memory-mapped file I/O)允许通过将磁盘块映射到内存中的页，将文件I/O视为常规内存访问。

最初使用需求分页demand paging读取文件。文件的页面大小page-sized portion of the file部分从文件系统读取到物理帧physical page。随后对文件的读/写被视为普通内存访问。

通过内存memory处理文件I/O而不是raed(), write()等系统调用来简化文件访问().还允许几个进程映射同一文件，允许共享内存中的page.访问模型如下：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201228194202001.png" alt="image-20201228194202001" style="zoom:80%;" />

Window系统中的memory-mapped共享内存模型：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201228194318328.png" alt="image-20201228194318328" style="zoom:80%;" />

## 七 核心态kernel 内存的分配Allocating Kernel Memory

##### 1.不适用分页机制处理数据和代码

和用户存储user memory策略不同。通常从自由内存池free-memory pool中分配：①内核请求存储不同大小结构的存储request memory for structures ——需要减少碎片fragmentation.②有些kernel内存需要是连续的contiguous(某些h/w设备与连续的物理内存交互)因此，许多系统不使用分页机制来处理内核代码和数据kernel code and data。

对于用户态的进程，申请资源时是以页为单位的，但是在内核态中却不是整页的申请。而内核态申请的内存空间如果大小不一的话会使得内存空间很凌乱、产生碎片，所以设计了伙伴算法buddy algorithm来分配空间给内核态中的进程

##### 2.伙伴系统Buddy System

从由物理连续页physically-contiguous pages组成的固定大小段fixed-size segment中分配内存。使用2的幂次的分配器power-of-2 allocator分配内存：①找到一块符合申请长度的连续块时，先不马上分出去；②把数据块一份为2；③判断分割后的数据块是否仍然满足申请的常速：如果满足，继续一分为二，如果不满足，则把分隔前的连续块分配出去。

伙伴算法分配出的空间固然有一点浪费，但是回收的空间却是整块的，有利于再分配：有内部碎片，但是没有外部碎片.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201228195417233.png" alt="image-20201228195417233" style="zoom:80%;" />

##### 3.Slab算法

另一种策略：①Slab是一个或多个连续排列的物理帧，②cache包含了一个或多个slabs，要求一个cache只能包含唯一的一种内核数据结构kernel data structure,③ 当cache被创建时，其内部填满了被标记为free的对象objects，当结构被存储进这些objects时，objects被标记为used。④如果slab内充满了used objects，下一个object会被分配给空empty的slab；如果没有空的slabs，会分配新的slab。

这种方式的好处是不会有碎片化，且能满足快速存储的请求fast memory request.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201228200050872.png" alt="image-20201228200050872" style="zoom:80%;" />

slab存储空间分配模型：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201228200209249.png" alt="image-20201228200209249" style="zoom: 67%;" />

##### 4.prepaging

是为了减少进程启动时发生的大量缺页page fault。在引用页之前，预先准备一个进程需要的所有或部分页。但是，如果预先准备好的页没有使用，I/O和内存就被浪费了。

假设s页是预先准备的，进程实际运行时的使用率为α，s*a是节省的缺页数若大于预先准备页的代价，则有利，否则则不划算。s×(1-a)是不必要的页，α接近0=>prepaging losses.

## 八 Other Considerations

##### 1.page size

必须考虑page size。它会影响到了：①碎片--适合小的page size；②页表的大小--适合大的页大小；③I/O负载overhead--适合较大的page size;④局部性locality--适合较小的page size。

##### 2.TLB reach

TLB Reach--是可以从TLB快表中访问的内存量。

TLB Reach=(TLB Size)*(Page Size)=表大小✖页大小

理想情况下，每个进程的工作集存储在TLB中，否则存在过高的缺页率。

增加页的大小--这可能会增加碎片，因为并非所有应用程序都需要较大的页面大小

提供多种页的大小--这使得需要更大页面大小的应用程序有机会使用它们，而不会增加碎片

##### 3.程序结构program structure

如数组int[128, 128]，实际的存储是将每一行存储为1页；所以同一列的各个元素实际上是都存储在不同的页中的。所以遍历的顺序会严重影响实际运行的时间。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201228204109670.png" alt="image-20201228204109670" style="zoom:80%;" />

##### 4.读写的内部锁 I/O interlock

I/O interLock --页有时必须锁定在内存中，如，用于从设备中复制文件的页面必须被锁定，以避免被页面替换算法选中被替换掉。

##### 5.用于I/O的物理帧为什么一定要在内存be in memory

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201228204528625.png" alt="image-20201228204528625" style="zoom:80%;" />

## 九 操作系统例子

##### 1.windoes XP

##### 2.Solaris