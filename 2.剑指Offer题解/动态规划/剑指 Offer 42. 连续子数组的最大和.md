# [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

>输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。要求时间复杂度为O(n)。
>
> 示例1:
>
>~~~
>输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
>输出: 6
>解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
>~~~

#### 方法一：连续最大子列和

这道题目是初学数据结构时的最大子列和的设计。可以直接分情况进行判断：

- 如果数组内没有正数，则任意两个子元素的和都小于数组中最大的那个元素，直接返回数组最大元素值即可。
- 否则，可以用tmpMax表示当前的最大值，若tmpMax<0，即当前连续子列和小于0，后面存在正数，加上该负数必定只会更小，所以可以重新将tmpMax归零，每次用max保存当前可得到的最大值即可。

~~~java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length==1) return nums[0];//只有一个元素
        boolean op=false;//标记正负
        int maxNum=nums[0],tmp=0,sum=Integer.MIN_VALUE;
        for(int num: nums){
            if(num>0) op=true;//数组内存在正数
            maxNum=Math.max(maxNum,num);//求数组的最大值
        }
        if(!op) return maxNum;//数组不存在正数，返回最大值
        for(int num: nums){
            tmp+=num;
            if(tmp<0) tmp=0;
            sum=Math.max(sum,tmp);
        }
        return sum;
    }
}
~~~

#### 方法二：动态规划

其实上面说的思路就是动态规划思路的一个更亲民的说法，可以通过动态规划将代码进行简化。

- 用dp[i]表示子列加到下标i的元素为止的最大子列和
- 每次需要判断前一个dp[i-1]是不是小于0，如果小于0，继续加上去只会更小，所以可以停止继续相加，直接将dp[i]=nums[i]
- 如果大于等于0，则可以继续加上当前值
- 每次不断更新所有子列和的最大值res，最终直接返回即可

~~~java
class Solution {
    public int maxSubArray(int[] nums) {
        //dp[i]表示加到下标i的最大子列和
        int[] dp=new int[nums.length];
        dp[0]=nums[0];
        int res=nums[0];
        for(int i=1;i<nums.length;i++){
            //前面和<0，则从当前值重新开始，否则，加上当前值
            if(dp[i-1]<0) dp[i]=nums[i];
            else dp[i]=nums[i]+dp[i-1];
            if(dp[i]>res) res=dp[i];//更新最值
        }
        return res;
    }
}
~~~