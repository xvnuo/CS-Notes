# [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

>请完成一个函数，输入一个二叉树，该函数输出它的镜像。
>
>例如输入：
>
>    		4
>    	 /   \
>      2     7
>     / \   / \
>    1   3 6   9
>    镜像输出： 
>    		4   
>    	/   \
>      7     2
>     / \   / \
>    9   6 3   1

还是用递归的方式来解决。考虑根结点，左子树和右子树，用两个中间变量来表示分别经过镜像翻转后的左右子树，然后直接在根节点上交换两个子树即可完成递归。

~~~java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if(root==null) return null;//空树
        //递归的中间变量--左右子树
        TreeNode left=mirrorTree(root.left);
        TreeNode right=mirrorTree(root.right);
        root.left=right;//交换左右子树
        root.right=left;
        return root;
    }
}
~~~

