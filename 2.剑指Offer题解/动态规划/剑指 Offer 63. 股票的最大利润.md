# [剑指 Offer 63. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

>假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？
>
>示例 1:
>
>输入: [7,1,5,3,6,4]
>输出: 5
>解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。

遍历数组。设置minP,表示当前最小股价，设置maxP,表示当前可以得到的最大利润，每次遍历更新minP和maxP,遍历到数组末尾即可找到最大利润maxP.

~~~java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices==null || prices.length<=1) return 0;
        int minP=prices[0];//最小股价
        int maxP=0;//最大利润
        for(int i=1;i<prices.length;i++){
            if(prices[i]<minP) minP=prices[i];
            if(prices[i]-minP>maxP) maxP=prices[i]-minP;
        }
        return maxP;
    }
}
~~~