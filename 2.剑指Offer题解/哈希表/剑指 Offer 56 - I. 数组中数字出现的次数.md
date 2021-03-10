# [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

>一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。
>
> 示例 1：
>
>输入：nums = [4,1,4,6]
>输出：[1,6] 或 [6,1]

用哈希表存放所有数组中元素出现的次数，然后第二次遍历数组，存储只出现一次的元素。

~~~java
class Solution {
    public int[] singleNumbers(int[] nums) {
        int[] res=new int[2];//存放结果
        int flag=0;
        HashMap<Integer,Integer> map=new HashMap<>();//哈希表
        for(int n: nums){//初始化哈希表
            map.put(n,map.getOrDefault(n,0)+1);
        }
        for(int n:nums){//存入结果数组中
            if(map.get(n)==1){
                if(flag==0){
                    res[0]=n;
                    flag++;
                }
                else res[1]=n;
            }
        }
        return res;
    }
}
~~~