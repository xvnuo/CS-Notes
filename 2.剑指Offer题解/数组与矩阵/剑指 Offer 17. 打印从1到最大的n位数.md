# [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

>输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。
>
>示例 1:
>
>输入: n = 1
>输出: [1,2,3,4,5,6,7,8,9]

感觉就是数组遍历？意义不大。

~~~java
class Solution {
    public int[] printNumbers(int n) {
        int len=(int)Math.pow(10,n)-1;
        int[] res=new int[len];
        for(int i=0;i<len;i++) res[i]=i+1;
        return res;
    }
}
~~~

