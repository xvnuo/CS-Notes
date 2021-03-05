# [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

>写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项（即 F(N)）。斐波那契数列的定义如下：
>
>~~~
>F(0) = 0,   F(1) = 1
>F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
>~~~
>
>斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。



## 方法1：迭代

和递归的差不多的思路，设f1, f2分别是前后两个数，下一次迭代时，f1和f2分别往后移动,f1变成f2, f2变成f1+f2，依次递推，最终返回f2即可。

~~~java
class Solution {
    public int fib(int n) {
        if(n==0) return 0;//n=0的情况
        if(n==1) return 1;//n=1的情况
        int f1=0,f2=1;
        for(int i=2;i<=n;i++){//不断迭代，f2是最终结果
            int tmp=f1;
            f1=f2;
            f2=f2+tmp;
            f2%=1000000007;//防止溢出
        }
        return f2;
    }
}
~~~



## 方法2：动态规划

是典型的动态规划问题，类似于递归的思路，但是动态规划可以将子问题的解缓存起来，从而避免重复求解子问题。

~~~java
public int Fibonacci(int n){
    if(n<=1) return n;//0和1的情况
    int[] dp=new int[n+1];//dp[i]表示n==i时的斐波那契数
    dp[0]=0;//初始化
    dp[1]=1;
    for(int i=2;i<=n;i++){//运用递推关系求dp[n]
        dp[i]=dp[i-1]+dp[i-2];
    }
    return dp[n];
}
~~~