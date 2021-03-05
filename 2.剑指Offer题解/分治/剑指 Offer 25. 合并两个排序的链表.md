# [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

>输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。
>
>**示例1：**
>
>```
>输入：1->2->4, 1->3->4
>输出：1->1->2->3->4->4
>```

注意题目中的两个链表都是递增顺序的，这个就很容易想到分治算法的解决思路上。

#### 方法一：逐个遍历

类似于两个链表相加、相乘等的思路，遍历两个链表，按照结点大小逐个添加到新的结果链表的尾部即可。

~~~java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        //任意一个为null,可以直接返回另一个
        if(l1==null) return l2;
        if(l2==null) return l1;

        ListNode res=new ListNode(-1),p=res;//结果链表
        while(l1!=null && l2!=null){
            if(l1.val<l2.val){//将较小者放到结果链表上
                p.next=new ListNode(l1.val);
                p=p.next;
                l1=l1.next;
            }
            else{//同理
                p.next=new ListNode(l2.val);
                p=p.next;
                l2=l2.next;
            }
        }
        if(l1!=null) p.next=l1;
        if(l2!=null) p.next=l2;
        return res.next;
    }
}
~~~

#### 方法二：分治算法

解决的代码其实就是链表用分而治之排序算法排序的“merge”部分的操作。首先判断两者是否有一个为空，若一个为空，可以直接返回另一个。然后按照l1.val和l2.val的值分别进行归并。一气呵成，非常奈斯。

~~~java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        //任意一个为null,可以直接返回另一个
        if(l1==null) return l2;
        if(l2==null) return l1;

        if(l1.val<l2.val){//归并到l1上面
            l1.next=mergeTwoLists(l1.next,l2);
            return l1;
        }
        else{//归并到l2上面
            l2.next=mergeTwoLists(l1,l2.next);
            return l2;
        }
    }
}
~~~