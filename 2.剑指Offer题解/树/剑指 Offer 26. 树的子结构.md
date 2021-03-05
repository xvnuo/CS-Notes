# [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

>输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)。B是A的子结构， 即 A中有出现和B相同的结构和节点值。
>
>例如:
>
>    给定的树 A:
>     3
>    / \
>    4  5
>      / \
>     1   2
>     
>    给定的树 B：
>       4 
>      /
>     1
>    返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。
>**示例 1：**
>
>```
>输入：A = [1,2,3], B = [3,1]
>输出：false
>```

二叉树涉及到结构上的比较的，一般都是通过递归的思路解决。思考递归细节时，可以按照题目思路将二叉树按照根节点、左子树、右子树三部分进行考虑。

- 若B为空，false--空树不是任意一个树的子结构
- 若A为空，false--B不空，A空，肯定不合题
- 当A不空且B也不空时
  - 比较A，B的左右子树和跟结点是不是一样，注意，传进isSame中的B必定不为空，但是B的左右子树可以为空，此时为true，对应于isSame中的第一个判断
  - 如果B的一些结点不空而A对应的结点空，必定false
  - 判断跟结点和以后子树中的值是否相同，返回false
  - 而后继续向左右子树递归
- 若A、B不在当前结点为子结构，则可继续向A的左子树和右子树上递归

~~~java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
        if(B==null) return false;//空树不是任意一个树的子结构
        if(A==null) return false;//B空，A不空，肯定错误
        //判断
        return isSame(A,B) || isSubStructure(A.left,B) || isSubStructure(A.right,B);
    }
    //判断两个树的是不是完全一样
    public boolean isSame(TreeNode A,TreeNode B){
        if(B==null) return true;
        if(A==null) return false;
        if(A.val!=B.val) return false;
return isSame(A.left,B.left)&&isSame(A.right,B.right);
    }
}
~~~