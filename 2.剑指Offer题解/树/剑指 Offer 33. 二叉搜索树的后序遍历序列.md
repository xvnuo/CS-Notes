# [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

>输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。参考以下这颗二叉搜索树：
>
>       5
>     / \ 
>    2   6
>      / \
>     1   3
>    
>    示例 1：
>    输入: [1,6,3,2,5]
>    输出: false
>    
>    示例 2：
>    输入: [1,3,2,6,5]
>    输出: true

二叉搜索树后序遍历，即左子树--右子树--根结点的遍历方式，同时左子树<根结点<右子树，利用这种结构特性可以归纳出解决方式：

- 若数组为空，则为空树，必定为true
- 若数组不为空，从左到右递归
  - 数组最后一个值必定是树根
  - 所有左子树的值都小于树根，对应于数组的前半部分，遍历数组找到第一个大于树根的位置index，该位置就是右子树的第一个元素，前面的元素都是左子树
  - 所有右子树的值都大于树根，从index开始向后遍历，若存在小于树根的元素，则该数组序列必定不是后序遍历序列，返回false
  - 若以上条件满足，以index为界，递归到左右子树，判断左右子树对应的数组序列部分是否也满足后序遍历的性质。

具体实现如下：

~~~java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        if(postorder==null) return true;//空数组
        return isPost(postorder,0,postorder.length-1);
    }
    public boolean isPost(int[] post,int left,int right){
        if(left>=right) return true;
        int index=left;
        //找到第一个大于根节点的节点值，即左子树边界
        while(index<right && post[index]<post[right]){
            index++;
        }
        //判断右半部分是不是全大于根结点
        for(int i=index;i<right;i++){
            if(post[i]<post[right]) return false;
        }
        //以index为界限，递归到左右子树
		return isPost(post,left,index-1) && isPost(post,index,right-1);
    }
}
~~~