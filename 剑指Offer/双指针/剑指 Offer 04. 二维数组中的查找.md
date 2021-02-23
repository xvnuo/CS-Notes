# [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

>在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
>
>示例:现有矩阵 matrix 如下：
>
>~~~
>[
>  [1,   4,  7, 11, 15],
>  [2,   5,  8, 12, 19],
>  [3,   6,  9, 16, 22],
>  [10, 13, 14, 17, 24],
>  [18, 21, 23, 26, 30]
>]
>~~~
>
>给定 target = 5，返回 true。给定 target = 20，返回 false。



## 方法1：双指针

矩阵是从左到右有序的，从上到下有序的，所以可以从右上角运用两个双指针向下面和左边遍历，时间复杂度为O(m+n)：

- 右上角的元素肯定是小于所有该列元素，大于所有该行元素，由此行指针初始化为0，列指针初始化为matrix[0].length
- 若该元素小于target，则该行元素肯定都小于target，行指针向下移动
- 若该元素大于target，则该列元素肯定都大于target，列指针向左移动
- 通过行指针和列指针的不断移动，最终可以找到该target对应的行列值
- 找不到返回false

~~~java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if(matrix==null || matrix.length==0) return false;
        int row=matrix.length,col=matrix[0].length;//行、列
        int m=0,n=col-1;//双指针，从右上角开始遍历
        while(m<row && n>=0){
            if(matrix[m][n]==target) return true;
            else if(matrix[m][n]<target) m++;
            else n--;
        }
        return false;//找不到
    }
}
~~~

从另一个角度看：将该矩阵绕着右上角旋转45度，其实整个矩阵就是一个二叉搜索树，即可按照这样的性质进行简化。

## 方法2：暴力查找

逐行逐列遍历二维矩阵，判断每个元素是否与目标值相同，时间复杂度O(m*n).

~~~java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0)  return false;
        int rows = matrix.length, col = matrix[0].length;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < col; j++) {
                if (matrix[i][j] == target) return true;
            }
        }
        return false;
    }
}
~~~