# [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

>给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
>
>百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
>
>例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)
>

简单递归

~~~java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==null) return null;
        if(root==p || root==q) return root;
        TreeNode p1=lowestCommonAncestor(root.left,p,q);
        TreeNode p2=lowestCommonAncestor(root.right,p,q);
        if(p1!=null && p2!=null) return root;
        if(p1==null) return p2;
        else return p1;
    }
}
~~~