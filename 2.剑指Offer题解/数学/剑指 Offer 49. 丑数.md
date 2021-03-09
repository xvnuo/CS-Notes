# [剑指 Offer 49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)

>我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。
>
> 示例:
>
>输入: n = 10
>输出: 12
>解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。

丑数--就是只包含质因子2，3，5的数，也就是说一定数量的2，3，5相乘可以得到丑数。题目要求找到第n个丑数，所以我们需要从1开始，逐个找到不确定数量的2，3，5相乘的最小值。可以利用“三指针”的方式，设置三个数值，标识当前最小的丑数可由上一个丑数乘以2，3，还是5得到，直到算到第n个即可。

~~~java
class Solution {
    public int nthUglyNumber(int n) {
        if(n<=0) return -1;
        int[] dp=new int[n];//dp[i]--第i+1个丑数
        dp[0]=1;
        int i1=0,i2=0,i3=0;
        for(int i=1;i<n;i++){
            dp[i]=Math.min(dp[i1]*2,Math.min(dp[i2]*3,dp[i3]*5));
            if(dp[i1]*2==dp[i]) i1++;
            if(dp[i2]*3==dp[i]) i2++;
            if(dp[i3]*5==dp[i]) i3++;
        }
        return dp[n-1];
    }
}
~~~