# [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

>0,1,···,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。
>
>例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。
>
>示例 1：
>
>输入: n = 5, m = 3
>输出: 3

类似于猴子选大王这种，可以用列表来不断进行删除，找出最后一个元素即可。

~~~java
class Solution {
    public int lastRemaining(int n, int m) {
        List<Integer> res=new ArrayList<>(n);
        for(int i=0;i<n;i++) res.add(i);//初始化
        int index=0;
        while(n>1){
            index=(index+m-1)%n;//每次进行计算
            res.remove(index);
            n--;
        }
        return res.get(0);//剩下的最后一个值
    }
}
~~~