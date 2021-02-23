# [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

>输入两个链表，找出它们的第一个公共节点。如下面的两个链表**：**[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)
>
>在节点 c1 开始相交。

可以继续使用集合的唯一性的性质，主要思路是：先将链A的所有结点都放入集合中，再遍历链B,逐一将B中结点add进集合，若add失败，说明集合中已经有了该对象，说明这是A和B的第一个公共结点，可以直接返回该结点。如果遍历完B还无这样的结点，说明A和B无公共结点，返回null.

思路比较清晰，就是时间复杂度和空间复杂度都太高了，qaq

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