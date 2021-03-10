# [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

>给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
>
>百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
>
>例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)
>

按照二叉搜索树的左右儿子和根结点之间的大小关系来进行递归求解。

~~~java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root==p || root==q) return root;
        if(p.val>root.val && q.val>root.val) 
            return lowestCommonAncestor(root.right,p,q);
        if(p.val<root.val && q.val<root.val) 
            return lowestCommonAncestor(root.left,p,q);
        else return root;
    }
}
~~~

