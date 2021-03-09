# [剑指 Offer 41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

>如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
>
>例如，
>
>[2,3,4] 的中位数是 3;  [2,3] 的中位数是 (2 + 3) / 2 = 2.5
>
>设计一个支持以下两种操作的数据结构：
>
>- void addNum(int num) - 从数据流中添加一个整数到数据结构中
>- double findMedian() - 返回目前所有元素的中位数。
>
>示例 1：
>
>~~~
>输入：
>["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
>[[],[1],[2],[],[3],[]]
>输出：[null,null,null,1.50000,null,2.00000]
>~~~

中位数即，将数组元素按照大小排序之后处于中间位置的元素，所以根本还是要进行排序操作。可以使用Java内置的sort方法排序，也可以利用优先队列构造出大顶堆和小顶堆，确保大顶堆和小顶堆的元素个数的绝对值不超过1，且大顶堆中的所有元素都比小顶堆小，即，大顶堆的最大元素，小于，小顶堆中的最小元素，那么中位数就取决于这两个堆顶元素。

具体实现如下：

~~~java
class MedianFinder {

    Queue<Integer> small,big;//构造两个堆
    public MedianFinder() {
        small=new PriorityQueue<>((v1,v2)->v1-v2);//小顶堆
        big=new PriorityQueue<>((v1,v2)->v2-v1);//大顶堆
    }
    
    public void addNum(int num) {
        if(big.size()<small.size()){//加入到大顶堆中
            small.offer(num);
            big.offer(small.poll());
        }
        else{//加入到小顶堆中
            big.offer(num);
            small.offer(big.poll());
        }
    }
    
    public double findMedian() {
        //相同大小则返回平均数，不同则返回小顶堆堆顶元素
        if(big.size()==small.size()) 
            return (double)(small.peek()+big.peek())/2.0;
        else return (double)small.peek(); 
    }
}
~~~

当然应该也可以使用List<>存储所有元素，每次寻求中位数时可以将其进行排序，然后直接返回。