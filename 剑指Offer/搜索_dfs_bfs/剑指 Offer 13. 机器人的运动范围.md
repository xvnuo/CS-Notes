# [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

>地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？
>
>**示例 1：**
>
>```
>输入：m = 2, n = 3, k = 1
>输出：3
>```

## 方法1：回溯法

使用回溯的方式，从坐标0，0开始搜索，设置visited来限制重复计算的格子。分别向左右上下分别进行暴力搜索，找出符合条件的结果。

~~~java
class Solution {
    int counter = 0;
    public int movingCount(int m, int n, int k) {
        boolean [][] visited = new boolean[m][n];
        movingTo(0,0,visited,k);
        return counter;
    }

    private void movingTo(int m, int n, boolean[][] visited, int k) {
      if (m<0||m>=visited.length||n<0||n>=visited[0].length)
          return;
        if (visited[m][n]) return;
        if(getNum(m)+getNum(n)>k) return;
        visited[m][n]=true;
        counter++;
        movingTo(m+1,n,visited,k);
        movingTo(m-1,n,visited,k);
        movingTo(m,n+1,visited,k);
        movingTo(m,n-1,visited,k);
    }
    int getNum(int n){//找坐标各个数位之和
        int result =0;
        while (n>0){
            result += n%10;
            n/=10;
        }
        return result;
    }
}
~~~

