# [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

>输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。
>
>示例 1：
>
>输入：target = 9
>输出：[[2,3,4],[4,5]]

使用滑动窗口的方式，left表示连续数组开始的位置，right表示连续数组终结的位置，每次tmpSum加上right，然后判断当前值是否大于target，如果大于，让左指针向右移动，即不断减去left，使得最终tmpSum<=target，如果等于target，则可以直接返回left到right之间的所有正整数，否则的话，右指针继续向右移动。

~~~java
class Solution {
    public int[][] findContinuousSequence(int target) {
        List<int[]> res=new ArrayList<>();//存放结果
        int left=1,right=1,tmpSum=0;
        while(right<target){
            tmpSum+=right;
            while(tmpSum>target){
                tmpSum-=left;
                left++;
            }
            if(tmpSum==target){//合题情况
                int[] arr=new int[right-left+1];
                for(int i=left;i<=right;i++){
                    arr[i-left]=i;
                }
                res.add(arr);//放入结果列表中
            }
            right++;//右指针继续向右移动
        }
        //将结果列表转换成结果数组
        int[][] t=new int[res.size()][];
        for(int i=0;i<res.size();i++) t[i]=rs.get(i);
        return t;
    }
}
~~~

