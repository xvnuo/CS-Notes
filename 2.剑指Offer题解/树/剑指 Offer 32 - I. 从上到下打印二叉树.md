# [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

>从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。例如:
>给定二叉树: `[3,9,20,null,null,15,7]`,
>
>```
> 	 3
>   / \
>  9  20
>    /  \
>   15   7
>```
>
>返回：
>
>```
>[3,9,20,15,7]
>```

就是二叉树的层序遍历，使用队列queue比较容易，从根结点逐个压入队列，每次弹出一个结点，并将节点值放入结果中，然后遍历到左右子树，如果左右子树不为空，则压入队列，这样压入再逐一弹出的队列其实就是按照从上到下，从左到右的层序遍历序列。

~~~java
class Solution {
    public int[] levelOrder(TreeNode root) {
        if(root==null) return new int[0];   //空树
        List<Integer> res=new ArrayList<>();//存放结果
        Queue<TreeNode> queue=new LinkedList<>();//队列
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode tmp=queue.poll();//顺序弹出
            res.add(tmp.val);//顺序弹出
            //压入左右子树
            if(tmp.left!=null) queue.offer(tmp.left); 
            if(tmp.right!=null) queue.offer(tmp.right);
        }
        //将list类型转换成数组并返回
        int[] arr=new int[res.size()];
        for(int i=0;i<res.size();i++) arr[i]=res.get(i);
        return arr;
    }
}
~~~