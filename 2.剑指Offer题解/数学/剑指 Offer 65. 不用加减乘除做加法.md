# [剑指 Offer 65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

>写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。
>
>示例:
>
>输入: a = 1, b = 1
>输出: 2

位运算的应用：

- 用(a&b)<<1表示a和b相加的进位
- 用a^b，即异或运算，表示无进位的加法结果
- 将上述两个数字相加，即为最终的结果，利用add方法不断进行迭代。

~~~java
class Solution {
    public int add(int a, int b) {
        if(b==0) return a;//一方为0，返回另一方
        int c=(a&b)<<1;//表示进位
        int d=a^b;//表示无进位的加法结果
        return add(d,c);
    }
}
~~~

