# [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

>用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )
>
>示例 1：
>
>~~~
>输入：
>["CQueue","appendTail","deleteHead","deleteHead"]
>[[],[3],[],[]]
>输出：[null,null,3,-1]
>~~~

## 方法1：利用栈的特性

用两个栈来实现一个队列的功能：

- 栈1专门负责push操作，push操作直接push到栈1中
- 栈2负责deleteHead操作，每次deleteHead操作，如果栈2是空的，则将栈1的所有元素都pop出来然后push进栈1，由于栈先进后出的特性，原本栈1的顶部元素就成了栈2的pop的优先级最低的元素，此时弹出的序列就变成了先进先出，符合队列的特性。此后如果还有push，仍然push进栈1，所以栈1存放的元素都比栈2要年轻，所以每次delete操作应该首先将栈2中的老元素弹出完，再进行以上操作。具体如下：

~~~java
class CQueue {
    Stack<Integer> s1;//栈1
    Stack<Integer> s2;//栈2
    public CQueue() {
        s1=new Stack<>();
        s2=new Stack<>();
    }
    
    public void appendTail(int value) {
        s1.push(value);//入栈
    }
    
    public int deleteHead() {
        if(!s2.empty()) return s2.pop();
        else{
            if(s1.empty()) return -1;
            while(!s1.empty()) s2.push(s1.pop());
            return s2.pop();
        }
    }
}
~~~

要注意java中栈的常见操作：

>Stack<Integer> s1=new Stack<>();//生成栈的方式
>
>s1.push(x);//push操作
>
>s1.pop(x);//pop操作，弹出并返回弹出的元素
>
>s1.peek();//不弹出，只返回栈顶元素