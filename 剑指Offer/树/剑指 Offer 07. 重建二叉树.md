# [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

>输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。
>
>例如，给出
>
>```
>前序遍历 preorder = [3,9,20,15,7]
>中序遍历 inorder = [9,3,15,20,7]
>```
>
>返回如下的二叉树：
>
>```
>    3
>   / \
>  9  20
>    /  \
>   15   7
>```

## 方法1：递归

前序遍历是根-左-右的顺序，中序遍历是左-根-右的顺序。对于每个前序数组，第一个元素一定是根结点，找到根节点值后可以构建根结点，然后再中序序列种找到根结点所在的下标，通过该下标即可将中序序列分成左中右三个部分，然后分别构建其左右子树，从而构建得出整个二叉树。

~~~java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
   return buildT(preorder, inorder, 0, inorder.length-1,0);
}
    public TreeNode buildT(int[] preorder, int[] inorder,int left,int right,int root){
        if(left>right) return null;
        TreeNode res=new TreeNode(preorder[root]);//根结点
        int index=0;
        for(int i=left;i<=right;i++){//根节点在中序序列中的位置
            if(inorder[i]==preorder[root]){
                index=i;
                break;
            }
        }
        //递归构建左右子树
res.left=buildT(preorder,inorder,left,index-1,root+1);
res.right=buildT(preorder,inorder,index+1,right,root+1+(index-left));
        return res;
    }
}
~~~