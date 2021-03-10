# [剑指 Offer 66. 构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)

>给定一个数组 A[0,1,…,n-1]，请构建一个数组 B[0,1,…,n-1]，其中 B[i] 的值是数组 A 中除了下标 i 以外的元素的积, 即 B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]。不能使用除法。
>
>示例:
>
>输入: [1,2,3,4,5]
>输出: [120,60,40,30,24]

就很巧妙，使用两个变量i和cur，第一次从左往右遍历，乘以每个元素左边的数，第二次从右往左遍历，乘以每个元素右边的数，最终返回结果即可。

~~~java
class Solution {
    public int[] constructArr(int[] a) {
        int[] res=new int[a.length];//存放结果
        for(int i=0,cur=1;i<a.length;i++){
            res[i]=cur;//先乘以左边的数，不包含自己
            cur*=a[i];
        }
        for(int i=a.length-1,cur=1;i>=0;i--){
            res[i]*=cur;//再乘以右边的数，不包含自己
            cur*=a[i];
        }
        return res;//返回结果
    }
}
~~~