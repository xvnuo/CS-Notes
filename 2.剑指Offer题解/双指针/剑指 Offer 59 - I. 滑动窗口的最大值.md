# [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

>给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。
>
>示例:
>
>~~~
>输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
>输出: [3,3,5,5,6,7] 
>解释: 
>
>  滑动窗口的位置                最大值
>---------------               -----
>[1  3  -1] -3  5  3  6  7       3
> 1 [3  -1  -3] 5  3  6  7       3
> 1  3 [-1  -3  5] 3  6  7       5
> 1  3  -1 [-3  5  3] 6  7       5
> 1  3  -1  -3 [5  3  6] 7       6
> 1  3  -1  -3  5 [3  6  7]      7
>
>~~~

#### 方法一：暴力运算

设置方法--找给定数组的最大值，然后每个滑动窗口都生成一个数组，调用上述方法找该数组的最大值，然后一个一个的存放到结果数组里面。

~~~java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int[] res=new int[nums.length-k+1];//存储结果
        if(nums.length==0) return new int[0];
        for(int i=0;i<nums.length-k+1;i++){//逐个窗口求最大值
            int[] temp=Arrays.copyOfRange(nums,i,i+k);
            res[i]=findMax(temp);
        }
        return res;
    }

    public int findMax(int[] nums){//求给定数组的最大值
        if(nums.length==0) return 0;
        Arrays.sort(nums);//排序
        return nums[nums.length-1];//返回最大值
    }
}
~~~

#### 方法二：单调队列

~~~java

~~~