# [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

>定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。
>
>**示例:**
>
>```
>输入: 1->2->3->4->5->NULL
>输出: 5->4->3->2->1->NULL
>```

翻转链表，既可以递归，也可以迭代。递归思路计算：首先判断只有一个结点或者链表为空的情况，可以直接返回head。递归梳理思路的时候可以把链表看成是两个结点，head和head.next，只需要把head.next向后.next链接到head上，形成一个环，然后再把head向后.next置为null，即可实现两个结点之间的断开，这是对基本的递归思路，结果链表的头结点就是reverseList(head.next)，通过循环以上的方式可以得到。最终返回res即可。非常巧妙。

~~~java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head==null || head.next==null) return head;
        ListNode res=reverseList(head.next);//新的链表头
        head.next.next=head;//翻转
        head.next=null;
        return res;
    }
}
~~~

