# [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

>输入两个链表，找出它们的第一个公共节点。如下面的两个链表：在节点 c1 开始相交。![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)
>
>
>
>示例 1：
>
>![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)
>
>输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
>输出：Reference of the node with value = 8
>输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。

可以利用HashSet集合中元素的互异性，首先将其中一个链表的结点逐个添加到HashSet中，然后将另外一个链表的结点逐个加入到Set中，若某个元素添加失败，说明链1中已经存在该Node，可以直接返回。

~~~java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Set<ListNode> nodes=new HashSet<>();//存储遍历的结点
        for(;headA!=null;headA=headA.next) nodes.add(headA);
        for(;headB!=null;headB=headB.next){//找到第一个相交元素
            if(!nodes.add(headB)) return headB;
        }
        return null;
    }
}
~~~

