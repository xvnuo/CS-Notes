# [剑指 Offer 60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

>把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。
>
> 你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。
>
>示例 1:
>
>输入: 1
>输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]

使用动态规划进行分析：

- n个骰子，最大值为6N，最小值为N，所以共有5N+1种取值情况
- 用res[5N+1]来存放所有的结果
- 每一种情况出现的概率是1.0/pow(6,n)
- 用dp【i】【j】表示i个骰子点数之和为j的情况的个数，n+1*6n+1的情况
  - 当i==1,即一个骰子，他的点数为1-6的情况各只有1种
  - 在n-1个骰子的基础上，再增加一个骰子出现点数和为s的结果有：f(n,s)=f(n-1,s-1)+f(n-1,s-2)+f(n-1,s-3)+...+f(n-1,s-6);
  - 所以可进行以下迭代：

~~~java
class Solution {
    public double[] dicesProbability(int n) {
        int len=5*n+1;//最大值n*6,最小值n*1
        double[] res=new double[len];//存放所有结果
        double eachP=1.0/Math.pow(6,n);//每一种情况的概率
        //dp[i][j]表示i个筛子点数之和为j的情况总数
        int[][] dp=new int[n+1][6*n+1];
        //一个筛子每个点数各有1种情况
        for(int i=1;i<=6;i++) dp[1][i]=1;
        for(int i=1;i<=n;i++){
            for(int j=i;j<=6*n;j++){
                for(int k=1;k<=6;k++){
                    if(j>=k) dp[i][j]+=dp[i-1][j-k];
                    if(i==n) res[j-i]=dp[i][j]*eachP;
                }
            }
        }
        return res;
    }
}
~~~