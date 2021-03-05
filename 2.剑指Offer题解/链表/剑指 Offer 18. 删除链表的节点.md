# [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

>给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。返回删除后的链表的头节点。**注意：**此题对比原题有改动
>
>示例 1:
>
>输入: head = [4,5,1,9], val = 5
>输出: [4,1,9]
>解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.

要特别注意头结点的处理方式：首先用一个while循环将和链表往下截，一直到头结点和给定值不同为止。遍历链表的过程中要注意next！=null的用意：必须有了被删除结点的前一个结点，才好删除该结点。

题目中说明了链表中结点的值各不相同，所以其实只需要找到目标值在链表中的位置并删除即可，开头只需要简单的判断就行了，不用像我一样大费周章。

~~~java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        //首先把头部中和val相同的值都给删除
        while(head!=null && head.val==val) head=head.next;
        if(head==null) return null;//若为空，则结束

        ListNode res=head;//此时head不为空，且head.val和val不同
        while(head.next!=null){
            if(head.next.val==val){//相同则删除
                head.next=head.next.next;
            }
            else{//若不同，继续向下遍历
                head=head.next;
            }
        }
        return res;
    }
}
~~~