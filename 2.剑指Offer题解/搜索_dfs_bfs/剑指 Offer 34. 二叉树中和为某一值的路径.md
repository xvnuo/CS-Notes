# [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

>输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。 
>
>示例:
>给定如下二叉树，以及目标和 sum = 22，
>
>              5
>             / \
>            4   8
>           /   / \
>          11  13  4
>         /  \    / \
>        7    2  5   1
>    返回:
>    [
>       [5,4,11,2],
>       [5,8,4,5]
>    ]

深度优先遍历：首先判断当前结点是否为空，如果为空停止继续向下回溯。而后sum-=root.val以及list.add(root.val)都是假设把当前结点的值放了上去。如果满足条件，则将该列表放入结果列表中去。然后继续向左右进行递归。具体的做法其实还是要仔细的，谁先谁后，这一点要格外的分清楚。

~~~java
class Solution {
    List<List<Integer>> res=new ArrayList<>();//存放结果
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        if(root==null) return res;//空树，直接返回
        dfs(root,sum,new ArrayList<>());
        return res;
    }
    public void dfs(TreeNode root,int sum,List<Integer> list){
        if(root==null) return;
        list.add(root.val);//假装把root添加上去
        sum-=root.val;
        if(sum==0 && root.left==null && root.right==null){
            res.add(new ArrayList<>(list));//合题则放到结果中
        }
        else{//向左右深度优先遍历
            dfs(root.left,sum,list);
            dfs(root.right,sum,list);
        }
        //移除
        list.remove(list.size()-1);
    }
}
~~~