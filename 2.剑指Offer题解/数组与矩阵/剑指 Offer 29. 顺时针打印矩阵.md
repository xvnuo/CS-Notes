# [剑指 Offer 29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

>输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。
>
>**示例 1：**
>
>```
>输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
>输出：[1,2,3,6,9,8,7,4,5]
>```

矩阵的顺时针遍历，非常经典的问题，可以设置四个变量up, down, left, right分别标记随着遍历，当前矩阵的可遍历到的范围，然后按照上--右--下--左--上的顺序逐行或逐列一边进行遍历，一边改变这些标记值，直到来到循环外边，遍历结束。

~~~java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if(matrix==null || matrix.length==0) return new int[0];
        int up=0,down=matrix.length-1,left=0,right=matrix[0].length-1;
        int[] res=new int[(down+1)*(right+1)];//存放结果
        int index=0;
        while(true){
            for(int i=left;i<=right;i++) res[index++]=matrix[up][i];
            if(++up>down) break;//上

            for(int i=up;i<=down;i++) res[index++]=matrix[i][right];
            if(--right<left) break;//右

            for(int i=right;i>=left;i--) res[index++]=matrix[down][i];
            if(--down<up) break;//下

            for(int i=down;i>=up;i--) res[index++]=matrix[i][left];
            if(++left>right) break;//左
        }
        return res;
    }
}
~~~

