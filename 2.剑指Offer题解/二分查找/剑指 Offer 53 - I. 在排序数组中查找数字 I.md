# [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

>统计一个数字在排序数组中出现的次数。
>
> **示例 1:**
>
>```
>输入: nums = [5,7,7,8,8,10], target = 8
>输出: 2
>```

使用二分法，先找到该数字在该排序数组中的位置，然后从该位置分别向两侧扩张。

~~~java
class Solution {
    public int search(int[] nums, int target) {
        int left=0,right=nums.length-1,count=0;//左右指针
        while(left<right){
            int mid=(right-left)/2+left;//中间位置
            if(nums[mid]>=target) right=mid;
            else left=mid+1;
        }
        //结束循环时，target一定大于等于nums[left]
        while(left<nums.length && nums[left]==target){
            count++;
            left++;
        }
        return count;
    }
}
~~~