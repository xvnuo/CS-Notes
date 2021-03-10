# [剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

>请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。若队列为空，pop_front 和 max_value 需要返回 -1
>
>示例 1：
>
>输入: 
>["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
>[[],[1],[2],[],[],[]]
>输出: [null,null,null,2,1,2]

#### 方法一：用数组模拟队列

~~~java
class MaxQueue {
    int[] q = new int[20000];//用数组模拟队列
    int begin = 0, end = 0,max=Integer.MIN_VALUE;//队列头尾
    public MaxQueue() {}//构造函数
    public int max_value() {//求队列最大值
        int res=-1;
        for(int i=begin;i<end;i++){//遍历数组
            res=Math.max(res,q[i]);
        }
        return res;
    }
    
    public void push_back(int value) {//push操作
        q[end++] = value;
    }
    
    public int pop_front() {//pop操作--弹出队头
        if (begin == end) {//队列空
            return -1;
        }
        return q[begin++];
    }
}
~~~