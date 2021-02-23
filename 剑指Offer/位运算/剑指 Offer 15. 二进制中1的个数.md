# [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

>请实现一个函数，输入一个整数（以二进制串形式），输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。
>
>示例 1：
>
>~~~
>输入：00000000000000000000000000001011
>输出：3
>解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
>~~~

## 方法1：位运算

n&(n-1)位运算可以将n的位级表示中最低的那一位1设置成0，不断将n中的1设置成0，直到n为0，即可算出n中的1的个数。

![img](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/image-20201105004127554.png)

~~~java
public int NumberOf1(int n) {
    int cnt = 0;
    while (n != 0) {
        cnt++;
        n &= (n - 1);
    }
    return cnt;
}
~~~

