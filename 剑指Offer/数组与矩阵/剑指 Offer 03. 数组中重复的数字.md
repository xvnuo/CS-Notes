#### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

------

#### 题目描述

>在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
>
>**示例 1：**
>
>```
>输入：
>[2, 3, 1, 0, 2, 5, 3]
>输出：2 或 3 
>```
>
>
>限制：2 <= n <= 100000
>

#### 方法一：哈希表查找

建立哈希表HashMap，保存每个数组元素出现的次数，遍历数组，一边更新每个元素在哈希表种的value，一边判断该元素是否出现了两次，返回第一次出现两次的元素。该方法空间和时间复杂度都比较大。需要注意哈希表HashMap的几个方法：

>int getOrDefault(int a, int b);  //如果Key值a在哈希表中，则返回a对应的value,如果不在，则往哈希表中放入Key值为a，value为b的pair。
>
>int get(int a);//直接返回键值a对应的value，若不存在该键值，则发生错误

~~~java
class Solution {
    public int findRepeatNumber(int[] nums) {
        HashMap<Integer,Integer> hm=new HashMap<>();
        for(int i=0;i<nums.length;i++){//哈希表辅助查找
            hm.put(nums[i],hm.getOrDefault(nums[i],0)+1);
            if(hm.get(nums[i])==2) return nums[i];
        }
        return -1;
    }
}
~~~

#### 方法二：原地交换元素位置

如果要求时间复杂度为O(N),空间复杂度为O(1)，则不能使用排序的方法，也不能使用额外的标记数组。数组元素在[0,n-1]范围内，所以可以将值为i的元素调整到第i个位置上进行求解。在调整过程中，如果第i个位置上已经有了一个值为i的元素，则该i值就是要找的重复出现的元素。

~~~java
public int findRepeatNumber(int[] nums){
    if(nums==null || nums.length==0) return -1;//数组空
    for(int i=0;i<nums.length;i++){
        //如果该数字和它的索引不相同--重复或者乱序的情况
        while(nums[i]!=i){
            if(nums[i]==nums[nums[i]]) return nums[i];
            int tmp=nums[nums[i]];//交换
            nums[nums[i]]=nums[i];
            nums[i]=tmp;
        }
    }
    return -1;
}
~~~