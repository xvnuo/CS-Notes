# [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

>给定一棵二叉搜索树，请找出其中第k大的节点。
>
> 示例 1:
>
>输入: root = [3,1,4,null,2], k = 1
>   3
>  / \
> 1   4
>  \
>   2
>输出: 4

比较固定的思路：二叉搜索树的中序遍历序列是有序的，所以可以通过递归找到该二叉搜索树的中序遍历序列，然后再获取其倒数第K个值。

~~~java
class Solution {
    List<Integer> res=new ArrayList<>();//存放中序遍历序列
    public int kthLargest(TreeNode root, int k) {
        inOrder(root);
        return res.get(res.size()-k);
    }
    //获取中序遍历序列
    public void inOrder(TreeNode root){
        if(root==null) return;//空树
        inOrder(root.left); //左子树
        res.add(root.val);  //根结点
        inOrder(root.right);//右子树
    }
}
~~~

同一种思路，如果是先遍历右子树，再分别遍历根节点和左子树，那么得到的就是中序遍历序列的逆序，即一个单调递减的序列。另外，可以增设一个count，来记录当前遍历到的结点是已经遍历到的第几个结点，当count==k时，我们自然而然就得到了第K大的结点，也就不用再存储整个中序遍历的序列了。实现如下：

~~~java
class Solution {
    int res=0,count=0;
    public int kthLargest(TreeNode root, int k) {
        inOrder(root,k);
        return res;
    }
    //获取中序遍历序列
    public void inOrder(TreeNode root,int k){
        if(root==null) return;
        inOrder(root.right,k);//先右子树
        if(++count==k){//对于当前根的处理
            res=root.val;
            return;
        }
        inOrder(root.left,k);//最后左子树
    }
}
~~~