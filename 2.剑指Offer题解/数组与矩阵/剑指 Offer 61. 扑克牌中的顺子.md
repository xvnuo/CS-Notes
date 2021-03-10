# [剑指 Offer 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

>从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
>
>示例 1:
>
>输入: [1,2,3,4,5]
>输出: True

题目的意思是0可以替换成任意数字，所以首先排序，找出0出现的次数，然后从zeroNum开始往后遍历，如果遇到后面数字种出现重复的数字，肯定不合题，返回false. 然后只需统计所有差值大于1的空隙，因为空隙可以用0来填补，所以如果空隙值小于等于0的个数，就可以形成顺子，否则就不行。

~~~java
class Solution {
    public boolean isStraight(int[] nums) {
        Arrays.sort(nums);//排序
        int zeroNum=0,diff=0;
        while(nums[zeroNum]==0) zeroNum++;//计算0的个数
        for(int i=zeroNum;i<nums.length-1;i++){
            //除了0之外还有相同的数
            if(nums[i]==nums[i+1]) return false;
            //计算所有的差值大于1的空隙
            diff+=(nums[i+1]-nums[i]-1);
        }
        return zeroNum>=diff;
    }
}
~~~

