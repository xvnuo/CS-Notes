# [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

>请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。
>
>例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
>
>    	  1
>    	/ \
>      2   2
>     / \ / \
>    3  4 4  3
>    但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
>    	  1
>    	/ \
>      2   2
>       \   \
>       3    3
>**示例 1：**
>
>```
>输入：root = [1,2,2,3,4,4,3]
>输出：true
>```

也是比较有意思的一个二叉树递归题。首先需要判断根结点是不是空的，如果是空的可以直接返回true。当根节点不是空的时，可以把原本一颗树的问题顺利转换成两个子树是否对称的问题，然后再去递归就方便的多了。

~~~java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root==null) return true;//空树
        //将一棵树的递归转换成两个子树的递归
        return isSame(root.left,root.right);
    }
    //判断两个子树是不是对称的
    public boolean isSame(TreeNode t1, TreeNode t2){
        //都为空
        if(t1==null && t2==null) return true;
        //一个为空，一个不空
        if((t1==null && t2!=null)||(t1!=null && t2==null)) return false;
        //根结点值不同
        if(t1.val!=t2.val) return false;
        //递归到左右子树
        return isSame(t1.left,t2.right) && isSame(t1.right,t2.left);
    }
}
~~~