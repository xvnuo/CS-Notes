# [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

>请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。例如:
>给定二叉树: [3,9,20,null,null,15,7],
>
>    	  3
>    	/ \
>      9  20
>        /  \
>       15   7
>    返回其层次遍历结果：
>    [
>      [3],
>      [20,9],
>      [15,7]
>    ]

仍然是用队列实现，在上一题的基础上添加一个int变量flag，用来记录是当前遍历到的层是奇数层还是偶数层，用list存储每一层的结点，如果是偶数层则需要将得到的列表从头到尾翻转，使用集合类的内置方法Collections.reverse(list)即可便捷实现。由此，层序遍历三部曲完成😄

~~~java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res=new ArrayList<>();//存放结果
        if(root==null) return res;
        Queue<TreeNode> queue=new LinkedList<>();//辅助队列
        queue.offer(root);
        int flag=0;//标记是否需要逆序
        while(!queue.isEmpty()){
            int size=queue.size();//记录每层结点数
            List<Integer> list=new ArrayList<>();//记录每层
            for(int i=0;i<size;i++){
                TreeNode tmp=queue.poll();//逐一弹出
                list.add(tmp.val);
                if(tmp.left!=null) queue.offer(tmp.left);
                if(tmp.right!=null) queue.offer(tmp.right);
            }
            if(flag%2==1) Collections.reverse(list);//逆序
            res.add(list);
            flag++;
        }
        return res;
    }
}
~~~