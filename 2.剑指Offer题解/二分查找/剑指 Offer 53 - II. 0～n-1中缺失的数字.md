# [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

>一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。
>
>示例 1:
>
>输入: [0,1,3]
>输出: 2

可以直接遍历，但是时间复杂度过大，明显要用二分法来进行优化。每次去中间值mid，如果mid==nums[mid]，已知mid左侧是有序的，所以mid左侧mid-0+1个元素肯定是从0~mid公mid+1个元素，每个都不缺的，即，所以缺少的数字肯定在右侧，令left=mid+1,否则的话，缺少的数字肯定是在mid或mid前面，令right=mid即可。

~~~java
class Solution {
    public int missingNumber(int[] nums) {
        int left=0,right=nums.length-1;//左右指针
        while(left<right){
            int mid=(right-left)/2+left;//中间值
            //若mid==nums[mid]，显然左侧被0~mid填满
            if(mid==nums[mid]) left=mid+1;
            else right=mid;//说明mid右侧没有问题
        }
        if(left==nums.length-1 && left==nums[left]) 
            return left+1;//从0-n-1都是有序的，少了一个n
        else return left;
    }
}
~~~