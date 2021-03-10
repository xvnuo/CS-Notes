# [剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

>输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。
>
> 示例 1：
>
>输入：nums = [2,7,11,15], target = 9
>输出：[2,7] 或者 [7,2]

可以使用哈希表记录每个元素的位置下标，然后遍历数组元素，判断target-num是否存在于哈希表中，然后进行返回即可。

注意题目中说该数组是有序的，所以可以使用双指针进行优化，使用左右两个指针，每次求两个元素的和，如果偏下，则可以让左指针右移，如果偏大，则可以让右指针左移，可以将整个时间复杂度降下来。实现如下：

~~~java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if(nums[0]>target) return new int[0];//无解
        int left=0,right=nums.length-1;//左右指针
        while(left<right){
            int tmp=nums[left]+nums[right];
            if(tmp==target) //得到结果值了，直接返回即可
                return new int[]{nums[left],nums[right]};
            if(tmp>target) right--;//右指针向左移动
            if(tmp<target) left++; //左指针向右移动
        }
        return new int[0];
    }
}
~~~