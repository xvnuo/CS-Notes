# [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

>定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。
>
>示例:
>
>~~~
>MinStack minStack = new MinStack();
>minStack.push(-2);
>minStack.push(0);
>minStack.push(-3);
>minStack.min();   --> 返回 -3.
>minStack.pop();
>minStack.top();      --> 返回 0.
>minStack.min();   --> 返回 -2.
>~~~

栈的设计问题，有两种思路，一是用链表，通过头插法不断将当前推入的结点插到链表最前面，产生和栈类似的后进先出的效果；二是仍然使用Stack结构，额外增加一个类内的变量min来记录每次更新结果后的最小值。

#### 方法一：头插法链表模拟栈结构

通过head=new Node(x,Math.min(head.min,x), head)，不仅实现了每次push操作更新当前栈内的最小值min，而且还通过head.next=head的方式，将每次插入的新结点放在了链表的头部，实现和栈一样的栈顶结构。

~~~java
class MinStack {

    Node head;//链表头，也是每次操作后的栈顶
    public MinStack() {}
    
    public void push(int x) {
        if(head==null){//若链表为空，直接初始化
            head=new Node(x,x,null);
        }
        else{//使用头插法，使得每次head都是栈顶
            head=new Node(x,Math.min(head.min,x),head);
        }
    }
    
    public void pop() {
        head=head.next;//直接往后移动
    }
    
    public int top() {
        return head.val;//直接返回栈顶值
    }
    
    public int min() {
        return head.min;//直接返回最小值
    }

    class Node{//Node类的具体设计
        int val;//结点值
        int min;//当前最小值
        Node next;
        Node(int val,int min,Node next){//构造函数
            this.val=val;
            this.min=min;
            this.next=next;
        }
    }
}
~~~

#### 方法二：使用两个栈,s1用于记录所有元素,s2用于记录最小值

每次更新操作时，将当前栈内的最小值同步更新到s2栈顶即可。但是用栈来实现栈，显得有点没有说服力，还是方法一更巧妙。

~~~java
class MinStack {
    Stack<Integer> s1,s2;

    public MinStack() {
        s1=new Stack<>();//创建栈
        s2=new Stack<>();
        //栈2用来记录最小值，初始先放入较大值
        s2.push(Integer.MAX_VALUE);
    }
    
    public void push(int x) {
        s1.push(x);
        //在s2栈顶不断更新当前栈内的最小值
        s2.push(Math.min(s2.peek(),x));
    }
    
    public void pop() {
        s1.pop();
        s2.pop();
    }
    
    public int top() {
        return s1.peek();
    }
    
    public int getMin() {
        return s2.peek();
    }
}
~~~