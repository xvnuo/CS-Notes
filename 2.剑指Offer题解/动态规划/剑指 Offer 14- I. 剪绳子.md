# [剑指 Offer 14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

>给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。
>
>**示例 1：**
>
>```
>输入: 2
>输出: 1
>解释: 2 = 1 + 1, 1 × 1 = 1
>```

## 方法1：动态规划

是典型的动态规划思想的应用。用dp[i]表示长度为i的绳子可得到的最大乘积。长度为i的绳子可以分成两份i-j和j。假设第一段不继续剪，第二段j的最大乘积要么是他自己，要么是dp[j].所以可以得到递推关系：

dp[i]=Math.max(dp[i], Math.max(j, dp[j]) * (i-j));

~~~java
class Solution {
    public int cuttingRope(int n) {
        int[] dp=new int[n+1];//dp[i]表示长度为i的绳子的最大乘积
        dp[1]=1;
        //dp[j]==dp[i]*(j-i)
        for(int i=2;i<=n;i++){
            for(int j=1;j<i;j++)
            dp[i]=Math.max(dp[i],Math.max(j,dp[j])*(i-j));
        }
        return dp[n];
    }
}
~~~



