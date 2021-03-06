# [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

>请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。
>
>示例 1：
>
>![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)
>
>输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
>输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]

复杂链表的复制，带有randowm和next指针，如果直接遍历链表一一复制的话，可能在设置random或next时，对应的结点还没有生成，还不存在，从而发生错误。可以利用哈希表的数据结构，一个原对象对应一个新生成的对象，首先遍历一遍链表，一一生成所有的结点，然后第二次遍历，找出所有的random和next关系，一一牵线搭桥，设置好结果链表的关系即可。

~~~java
class Solution {
    public Node copyRandomList(Node head) {
        if(head==null) return null;
        HashMap<Node,Node> map=new HashMap<>();
        //遍历链表，将各个结点的值逐一生成放到图中
        for(Node p=head;p!=null;p=p.next){
            map.put(p,new Node(p.val));
        }
        for(Node p=head;p!=null;p=p.next){
            map.get(p).next=map.get(p.next);//next指针设置
            map.get(p).random=map.get(p.random);//random设置
        }
        return map.get(head);//返回新头        
    }
}
~~~