# [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

>输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。
>
>例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。
>
>**示例：**
>
>```
>给定一个链表: 1->2->3->4->5, 和 k = 2.
>
>返回链表 4->5.
>```

链表遍历的常见方法--快慢指针。也是求链表倒数第几个结点这类问题中的常见方法。

可以设置一个快指针一个慢指针，快指针比慢指针先走K步，然后让快慢指针一起走，当块指针走到链表尾部时，此时的慢指针比快指针少走了K步，所以慢指针刚好指向倒数第K个结点。

~~~java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode fast=head,slow=head;//快慢指针
        for(int i=0;i<k;i++) fast=fast.next;//快指针先走
        while(fast!=null){//快慢指针一起走
            fast=fast.next;
            slow=slow.next;
        }
        return slow;
    }
}
~~~