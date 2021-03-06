# [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

>从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。例如:
>给定二叉树: `[3,9,20,null,null,15,7]`,
>
>    	 3
>    	/ \
>      9  20
>        /  \
>       15   7
>    返回其层次遍历结果：
>    [
>      [3],
>      [9,20],
>      [15,7]
>    ]

二叉树层序遍历，只不过需要分层，使用队列结构时，首先将根节点压入队列，用int变量size来记录队列每层的结点数，然后再按照前面的经验来做即可。

~~~java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res=new ArrayList<>();//存放结果
        if(root==null) return res;//空树情况
        Queue<TreeNode> queue=new LinkedList<>();//辅助的队列
        queue.offer(root);
        while(!queue.isEmpty()){
            int size=queue.size();//记录二叉树每层的结点数
            List<Integer> list=new ArrayList<>();//存放每层
            for(int i=0;i<size;i++){
                TreeNode tmp=queue.poll();//逐一弹出
                list.add(tmp.val);
                //压入左右子树，即下一层
                if(tmp.left!=null) queue.offer(tmp.left);
                if(tmp.right!=null) queue.offer(tmp.right);
            }
            res.add(list);
        }
        return res;
    }
}
~~~