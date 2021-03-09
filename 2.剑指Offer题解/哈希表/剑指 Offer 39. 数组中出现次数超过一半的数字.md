# [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

>数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。你可以假设数组是非空的，并且给定的数组总是存在多数元素。
>
>示例 1:
>
>输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
>输出: 2

#### 方法一：哈希表标记数字出现次数

遍历数组，同时用哈希表来记录数组中每个元素出现的次数，随着遍历不断更新，如果发现某个数字出现的次数符合要求则返回。思路比较简单但是内存空间和时间复杂度都比较高。

~~~java
class Solution {
    public int majorityElement(int[] nums) {
        HashMap<Integer,Integer> map=new HashMap<>();//哈希表
        for(int n: nums){//遍历数组
            map.put(n,map.getOrDefault(n,0)+1);//更新哈希值
            if(map.get(n)>nums.length/2) return n;
        }
        return -1;
    }
}
~~~

#### 方法二：排序

动动脑筋想一想，如果某个元素出现的次数超过数组长度的一半，将数组排序，那么无论该元素是数组中最小的元素、最大的元素还是介于最大和最小元素之间的，数组正中间位置的元素一定是该元素。所以可以直接返回数组的中间元素。

~~~java
class Solution {
    public int majorityElement(int[] nums) {
        return nums[nums.length/2];//返回中间结果
    }
}
~~~