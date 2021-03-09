# [剑指 Offer 46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

>给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。
>
> 示例 1:
>
>输入: 12258
>输出: 5
>解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"

类似于青蛙跳台阶，仍然可以用动态规划来解决：

- 让dp[i]表示[0,i-1]范围内的数字可翻译的方法数，则dp[0]=0, dp[1]=1;
- 对于一个字符，首先将它前面两个字符看成是独立的字符，所以必有dp[i]=dp[i-1]
- 然后将它的前面两个字符当作一个整体看待 ，如果ch[i-2]==1或者ch[i-2]= =2&&ch[i-1]<6，则前面两个字符可以组合成一个介于l和z字符中间的一个字符，那么dp+=dp[i-2]即可。

~~~java
class Solution {
    public int translateNum(int num) {
        //转换成字符数组
        char[] ch=String.valueOf(num).toCharArray();
        //dp[i]表示下标[0,i-1]范围的字符的翻译方法数
        int[] dp=new int[ch.length+1];
        dp[0]=1;
        dp[1]=1;
        for(int i=2;i<=ch.length;i++){
            dp[i]=dp[i-1];
            if(ch[i-2]=='1' || (ch[i-2]=='2'&&ch[i-1]<'6')){
                dp[i]+=dp[i-2];
            }
        }
        return dp[ch.length];
    }
}
~~~