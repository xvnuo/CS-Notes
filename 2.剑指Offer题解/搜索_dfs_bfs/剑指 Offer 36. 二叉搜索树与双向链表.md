# [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

>输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。为了让您更好地理解问题，以下面的二叉搜索树为例：
>
>![img](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)
>
>我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。
>
>下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。
>
>![img](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png) 
>
>特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。
>

利用二叉树的结构特性和结点值特性：中序遍历的节点值序列是单调递增的。所以可以使用dfs进行中序遍历，dfs(left)，dfs(right)，中间对当前遍历到的根节点进行处理。对每个结点，设置一个pre结点作为当前节点中序遍历到的上一个结点，改变这两个结点之间左右的相互指向即可。特别注意，当pre为空的时候，即遍历到整棵树最左边的那个结点时，head此时还为null，所以此时head应该设置为没有左儿子，即直接让head=root即可。

通过这个题目，感受到自己对综合性的题目，比如二叉树+中序遍历+深度优先搜索这样结合起来的问题处理的还不是很熟悉，主要是没有仔细耐心梳理一下整个的思路和代码布局设计。

~~~java
class Solution {
    Node head,pre;
    public Node treeToDoublyList(Node root) {
        if(root==null) return null;//空树
        dfs(root);
        head.left=pre;
        pre.right=head;
        return head;
    }
    public void dfs(Node root){
        if(root==null) return;//停止继续向下深度优先搜索
        dfs(root.left);//递归到左子树
        //对当前结点
        root.left=pre;          //改变左指向
        if(pre==null) head=root;//改变右指向
        else pre.right=root;
        pre=root;//更新pre-->成为下一次dfs的前一个结点
        dfs(root.right);//递归到右子树
    }
}
~~~

