# [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

>输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。
>
>**示例 1：**
>
>```
>输入：head = [1,3,2]
>输出：[2,3,1]
>```



## 方法1：数组逆序存储

常规方法，直接按照题目要求，首先遍历一遍链表，求得链表长度，生成对应长度的数组，然后再次从头遍历该链表，并依次将链表结点值逆序放入数组，即可实现从尾到头把链表的结果存入数组中。

~~~java
class Solution {
    public int[] reversePrint(ListNode head) {
        int length=0;
        ListNode p=head;
        while(p!=null){//遍历链表求链表长度
            length++;
            p=p.next;
        }
        int[] res=new int[length];
        while(head!=null){//遍历链表，逆向装入数组中
            res[--length]=head.val;
            head=head.next;
        }
        return res;
    }
}
~~~

## 方法2：递归

利用i在不同子递归中的值来标记最终存入结果数组中的索引。非常巧妙。

~~~java
class Solution{
    int len=0;//记录长度
    int i=0;//记录递归中的索引
    int[] res;//存放链表逆序结果
    public int[] reversePrint(ListNode head){
        if(head==null){
            len=i;
            return new int[len];
        }
        i++;
        res=reversePrint(head.next);
        i--;
        res[len-i-1]=head.val;
        return res;
    }
}
~~~

另一种递归思路：要逆序打印链表1-2-3，可以先逆序打印链表2-3，最后再打印第一个结点，而链表2-3可以递归成，先打印3，再打印2，得到结果3，2，1.最终把结果转换成数组即可。过程如下：

~~~java
public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
    ArrayList<Integer> ret = new ArrayList<>();
    if (listNode != null) {
        ret.addAll(printListFromTailToHead(listNode.next));
        ret.add(listNode.val);
    }
    return ret;
}
~~~

## 方法3：使用栈

利用栈先进后出的特性，先将链表结点一个一个push到栈中，再一个一个弹出放到结果数组中。这里可以先放到ArrayList中，再转化成数组。

~~~java
public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
    Stack<Integer> stack = new Stack<>();
    while (listNode != null) {
        stack.add(listNode.val);
        listNode = listNode.next;
    }
    ArrayList<Integer> ret = new ArrayList<>();
    while (!stack.isEmpty())
        ret.add(stack.pop());
    return ret;
}

~~~