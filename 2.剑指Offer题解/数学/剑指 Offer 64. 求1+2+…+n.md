# [剑指 Offer 64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)

>求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。
>
>示例 1：
>
>输入: n = 3
>输出: 6

可以利用这个方法本身的功能进行递归。

- 如果n==0，可以终止递归，返回0
- 否则的话，返回n+sumNums(n-1)，即把原计算拆分成两个部分n和剩余部分1~n-1的和即可。

~~~java
class Solution {
    public int sumNums(int n) {
        if(n==0) return 0;
        else return n+sumNums(n-1);//向下降解递归
    }
}
~~~