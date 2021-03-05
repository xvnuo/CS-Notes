# [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

>输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。
>
>**示例：**
>
>```
>输入：nums = [1,2,3,4]
>输出：[1,3,2,4] 
>注：[3,1,2,4] 也是正确的答案之一。
>```

典型的双指针应用题目。可以从两端开始遍历，利用while循环快速找到左半部分第一个偶数和右半部分第一个奇数，交换两个数字，重复这个过程知道两个指针相遇，即可使得前半部分是奇数，后半部分都是偶数。

~~~java
class Solution {
    public int[] exchange(int[] nums) {
        int left=0,right=nums.length-1;//左右指针
        while(left<right){
            //逐个找到前半部分的偶数
            while(left<right && nums[left]%2==1) left++;
            //逐个找到后半部分的奇数
            while(left<right && nums[right]%2==0) right--;
            if(left<right){//交换，使得前面是奇数，后面是偶数
                int tmp=nums[left];
                nums[left]=nums[right];
                nums[right]=tmp;
            }
        }
        return nums;
    }
}
~~~