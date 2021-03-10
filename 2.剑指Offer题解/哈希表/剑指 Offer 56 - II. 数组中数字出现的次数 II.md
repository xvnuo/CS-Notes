# [剑指 Offer 56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

>在一个数组 nums 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。
>
>示例 1：
>
>输入：nums = [3,4,3,3]
>输出：4

仍然用哈希表记录每个元素在数组中出现的次数，就是时间复杂度和空间复杂度太高了，也许可以尝试一下位运算。

~~~java
class Solution {
    public int singleNumber(int[] nums) {
        Map<Integer,Integer> map=new HashMap<>();//哈希表
        for(int n: nums){
            if(map.containsKey(n)) map.put(n,map.get(n)+1);
            else map.put(n,1);
        }
        for(int n: nums){
            if(map.get(n)==1) return n;
        }
        return 0;
    }
}
~~~