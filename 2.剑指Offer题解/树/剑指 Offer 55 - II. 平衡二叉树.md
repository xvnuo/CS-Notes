# [剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

>输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。
>
> 
>
>示例 1:
>
>给定二叉树 [3,9,20,null,null,15,7]
>
>    	  3
>    	/ \
>      9  20
>        /  \
>       15   7
>    返回 true 。

使用常规的递归方法来进行解决。

~~~java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root==null) return true;
        if(Math.abs(getDepth(root.left)-//当前根结点平衡
                    getDepth(root.right))>1) return false;
        return isBalanced(root.left)//左右子树都平衡
            					&&isBalanced(root.right);
    }
    public int getDepth(TreeNode root){//求结点深度
        if(root==null) return 0;
        return 1+Math.max(getDepth(root.left),
                          getDepth(root.right));
    }
}
~~~

